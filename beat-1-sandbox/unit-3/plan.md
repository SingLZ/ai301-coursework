# Plan: #68, keyword search raises `ZeroDivisionError` when the index is empty

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68
My repro: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5850748432
(`main` at `2f4e82f`, Python 3.11.7, macOS 26.6.2 arm64, `rank-bm25` 0.2.2)

## Diagnosis

`KeywordSearcher.index()` builds a `BM25Okapi` from the tokenized corpus
every time it runs, even when `chunks` is empty. With zero documents,
`rank_bm25` divides by the corpus size while it initializes, and that
raises. `search()` already guards the empty case; `index()` does not.

The repro evidence I rely on:

Run 1 (`KeywordSearcher().index([])`) fails inside the `BM25Okapi`
constructor, reached from `index()`:

```
  File ".../rag/retriever/keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
  ...
  File ".../site-packages/rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero
```

Run 3, the control, shows that the empty list is the only trigger. One
chunk indexes fine, and `search()` on a searcher that was never indexed
already returns `[]`:

```
index([1 chunk]) -> OK, bm25 = BM25Okapi
search("python") -> [{'id': 1, 'text': 'python programming', 'bm25_score': -0.2746530721670274}]
search() before index() -> []
```

So the bug is on the write path, at `keyword_search.py` line 25, not in
`search()` and not in how `rank_bm25` scores a real corpus. Run 2 shows
the covering test, `test_empty_index`, is `XFAIL` (strict) for this
reason.

## Scope

In scope:
- An empty-corpus guard in `KeywordSearcher.index()`. When `chunks` is
  empty, store the empty list, set `self.bm25 = None`, log, and return
  without constructing `BM25Okapi`. `search()`'s existing guard then
  returns `[]`.
- Removing the `@pytest.mark.xfail(strict=True, ...)` marker from
  `test_empty_index`, as `docs/CONTRIBUTING.md` requires for a seeded
  bug.
- One new unit test: re-indexing with `[]` after a non-empty index
  leaves the searcher returning `[]`, not stale results.

Not in scope:
- `search()`, `_tokenize()`, and scoring. They behave correctly in my
  control run.
- `rag/retriever/hybrid.py`. It only calls `search()`, which already
  handles an empty index.
- Patching or upgrading `rank-bm25`.
- Corpora that are non-empty but tokenize to nothing (see Risks).
- Any lint or type cleanup elsewhere in the file.

## Files I'll touch

- `rag/retriever/keyword_search.py`: `index()` only.
- `tests/unit/test_keyword_search.py`: drop the xfail marker on
  `test_empty_index`, and add one re-index test.

## Approach

1. In `index()`, after `self.chunks = chunks`, add:
   `if not chunks:` set `self.bm25 = None`, log
   `keyword_index_empty`, and `return`. Setting `bm25` to `None`
   (rather than leaving it alone) matters when an existing searcher is
   re-indexed with nothing. Otherwise the old `BM25Okapi` would still
   be there while `self.chunks` is empty.
2. Leave the non-empty path exactly as it is.
3. Remove the xfail marker from `test_empty_index`.
4. Add `test_reindex_with_empty_clears_index`: index two chunks, then
   index `[]`, then assert `search("python") == []`.
5. Run `make check` and `make test-unit`.

## Test plan

Re-run my Unit 2 repro steps against the fix, from the same venv:

1. Run 1, `KeywordSearcher().index([])`. Before: the
   `ZeroDivisionError` traceback above. Expected after: no traceback,
   and the script exits 0. I'll also print `s.search("python")`, which
   should print `[]`.
2. Run 2, `pytest "tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index" -rx`.
   Before: `1 xfailed`. Expected after, with the marker removed:
   `1 passed`. If I leave the marker in, strict mode should report
   `XPASS(strict)` as a failure, which is a second sign the fix took.
3. Run 3, the control. Expected after: the same three lines as before,
   with the same `bm25_score`. This shows the non-empty path is
   unchanged.
4. The whole file, `pytest tests/unit/test_keyword_search.py -q`.
   Before: `16 passed, 1 xfailed`. Expected after: `18 passed` (16, plus
   the former xfail, plus the new re-index test).

## Risks and unknowns

- Not fixed here, and I checked it: a corpus that is non-empty but
  tokenizes to nothing (for example `[{"id": 1, "text": "   "}]`) also
  raises `ZeroDivisionError` on my machine, at a different line in
  `rank_bm25` (`average_idf = idf_sum / len(self.idf)`). That is a
  different trigger than #68 names, so I'm leaving it out and will
  mention it on the thread as a possible follow-up.
- I haven't checked whether any caller relies on `self.bm25` being a
  `BM25Okapi` after `index()` returns. The only caller I found in the
  repo, `rag/retriever/hybrid.py`, goes through `search()`.
- I've only tested Python 3.11.7 on macOS arm64.

## Deviations

Built on branch `fix/68-empty-index-guard`, commit `029884d`. The fix,
the files, and the scope are as planned. One change to the test:

- **The re-index test now also asserts `searcher.bm25 is None`.** As
  first written, `test_reindex_with_empty_clears_index` only asserted
  that `search()` returns `[]` after `index([])`. While reviewing it, I
  noticed that this passes even without the `self.bm25 = None` line,
  because `search()` also returns early when `self.chunks` is empty. So
  the test didn't check what its name claims. I added the assert on
  `bm25`. Then I temporarily removed `self.bm25 = None` from the guard
  and reran the file: the new test failed (`1 failed, 17 passed`).
  After I put the line back, all 18 passed.
- One small addition the plan didn't spell out: the guard has a
  two-line comment explaining why it exists.

Test plan results match what the plan expected: `index([])` exits 0
and `search()` prints `[]`, `test_empty_index` passes, the control
prints the same three lines with the same `bm25_score`, and the file
goes from `16 passed, 1 xfailed` to `18 passed`. `make check` is clean,
and `make test-unit` shows `377 passed, 52 xfailed`. The posted plan is
still accurate, so no follow-up comment is needed on the issue.
