# Unit 3 — Plan and Build

## Posted upstream

### GitHub username

SingLZ

### Plan comment

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5922365145

PLAN:

**Diagnosis.** My traceback puts the division in `BM25Okapi`'s constructor (`rank_bm25.py` line 52, `self.avgdl = num_doc / self.corpus_size`), reached from `keyword_search.py` line 25 in `index()`. My control run shows the empty list is the only trigger: one chunk indexes fine, and `search()` on a never-indexed searcher already returns `[]`. So `index()` is missing the empty-corpus guard that `search()` has.

**Change.** One function, `KeywordSearcher.index()` in `rag/retriever/keyword_search.py`. When `chunks` is empty, it stores the empty list, sets `self.bm25 = None`, logs, and returns without building `BM25Okapi`. Setting `bm25` to `None` covers re-indexing an existing searcher with nothing, so no stale index is left behind. In `tests/unit/test_keyword_search.py`, I'll remove the strict xfail marker on `test_empty_index` (per CONTRIBUTING) and add one test for that re-index case.

**Not touching:** `search()`, scoring, `hybrid.py`, or the `rank-bm25` dependency.

**Test.** I'll re-run my three repro runs. `index([])` should return without raising, and `search()` should give `[]`. `test_empty_index` should go from `xfailed` to `passed`. The one-chunk control should print the same output as before. The test file should go from `16 passed, 1 xfailed` to `18 passed`.

**Not yet checked / out of scope.** A non-empty corpus whose text is only whitespace also raises `ZeroDivisionError` on my machine, but at a different line in `rank_bm25` (`average_idf`). That's a different trigger than this issue names, so I'm leaving it out of this fix and flagging it here as a possible follow-up. I also haven't checked whether any caller relies on `self.bm25` being set after `index()`; the only caller I found, `hybrid.py`, goes through `search()`. I've only tested on Python 3.11 on macOS.

## Your branch

### Branch

fix/68-empty-index-guard

### Evidence

Unit 2 repro steps re-run in the same venv (Python 3.11.7, macOS arm64,
`rank-bm25` 0.2.2). Before = `main` at `2f4e82f`; after = branch
`fix/68-empty-index-guard` at `029884d`. Structlog's timestamped log
lines are left out of the control output, and home-directory paths are
shortened to `~`.

**Before (unfixed, `2f4e82f`)**

```
$ git rev-parse --short HEAD
2f4e82f

$ .venv/bin/python -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search(\"python\"))"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "~/pathreview-ai301-fa26-s3/rag/retriever/keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "~/pathreview-ai301-fa26-s3/.venv/lib/python3.11/site-packages/rank_bm25.py", line 83, in __init__
    super().__init__(corpus, tokenizer)
  File "~/pathreview-ai301-fa26-s3/.venv/lib/python3.11/site-packages/rank_bm25.py", line 27, in __init__
    nd = self._initialize(corpus)
         ^^^^^^^^^^^^^^^^^^^^^^^^
  File "~/pathreview-ai301-fa26-s3/.venv/lib/python3.11/site-packages/rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
                 ~~~~~~~~^~~~~~~~~~~~~~~~~~
ZeroDivisionError: division by zero
exit: 1

$ .venv/bin/python -m pytest "tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index" -rx

=========================== short test summary info ============================
XFAIL tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index - issue #68 (manifest H-01): BM25 keyword search raises ZeroDivisionError on an empty index
============================== 1 xfailed in 0.23s ==============================

$ control: one chunk, and search() before index()
index([1 chunk]) -> OK, bm25 = BM25Okapi
search("python") -> [{'id': 1, 'text': 'python programming', 'bm25_score': -0.2746530721670274}]
search() before index() -> []

$ .venv/bin/python -m pytest tests/unit/test_keyword_search.py -q
........x........                                                        [100%]
16 passed, 1 xfailed in 0.17s
```

**After (fixed, `029884d`)**

```
$ git rev-parse --short HEAD
029884d

$ .venv/bin/python -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search(\"python\"))"
2026-09-30 18:15:35 [info     ] keyword_index_empty
2026-09-30 18:15:35 [warning  ] keyword_search_empty_index
[]
exit: 0

$ .venv/bin/python -m pytest "tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index" -rx

============================== 1 passed in 0.19s ===============================

$ control: one chunk, and search() before index()
index([1 chunk]) -> OK, bm25 = BM25Okapi
search("python") -> [{'id': 1, 'text': 'python programming', 'bm25_score': -0.2746530721670274}]
search() before index() -> []

$ .venv/bin/python -m pytest tests/unit/test_keyword_search.py -q
..................                                                       [100%]
18 passed in 0.16s
```

## Eval iterations

### Run history

1. Run 1 (full, 20 packages): `agreement: 19/20 scored items  (bar: 18/20: PASS)`,
   with `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
   This is the run saved in `eval-run.txt`. I made no partial `--only`
   runs after it, because it already met the bar and the category floor.

### Package analysis

**pkg-14** (zellij-org/zellij#5174, category clear-accept). Gold: **accept**.
My rubric: **reject**, failed on `executable` only. Every other required
check passed.

The grader's evidence for the fail was the plan's Files line: "exact
functions to be pinned in the PR after tracing the query issuance with
debug logs". My `executable` check's pass condition says the plan must
name "where the change goes (a file, function, branch, or call site)".
It also fails a plan whose "first step is to investigate or profile and
decide later". pkg-14 names the area ("the client attach/reattach path
in `zellij-server` (session connection handling)") and commits to one
approach: drain the OSC responses before pane input is wired, bounded to
OSC response patterns. But it defers the exact functions to a trace, and
the grader read that deferral as the "investigate and decide later"
case.

I think gold is right. The plan has chosen what to do and roughly where.
The deferral is a lookup, not an open design choice. That's different
from pkg-17 ("gocui? tcell? not sure") or pkg-18 ("a recover()
safety net somewhere around linter execution"). My check doesn't separate "the location is narrowed to a
code path and the last lookup is deferred" from "the location is
unknown". It treats any deferred location as unnamed.

### Check rationale

Quoted exactly from `tools/plan-check/rubric.md`:

| executable | The plan's files, functions, or sites named, and its approach or change steps. | A stranger holding only this package could start the change without asking the author anything: the plan names where the change goes (a file, function, branch, or call site) and commits to one approach. Fail if the location is "somewhere" or unnamed, the approach is a choice left open ("upstream or vendored, whichever is easier", "gocui? tcell? not sure"), or the plan's first step is to investigate or profile and decide later. | required |

Why it reads this way. I started from the lecture's "a stranger could
not start executing it" failure family, plus the three unbuildable
packages in the calibration and eval sets: calib-02 ("poke around the
editor components this weekend"), pkg-17 and pkg-18. The examples in the fail
list come from those packages and their gold-label notes on purpose. The grader then has
concrete shapes to match, instead of an adjective like "vague". I
rejected a structural version ("the plan has a Files section"). calib-01
is a gold accept with no headings at all, just a "Change:" sentence
that names `pkg/gui/controllers/sync_controller.go`. A section-based
check would have held it for its shape. So the check asks whether a
location and one approach exist, not whether a heading does.

### Trade-offs

The check gives up pkg-14. Making "names where the change goes" strict
is what holds pkg-10, pkg-17 and pkg-18 (all three unbuildable
packages matched in run 1). But it also rejected pkg-14, a gold accept
that narrows the location to the zellij-server reattach path and leaves
the exact functions to a trace. The run showed this directly: pkg-14's
note column reads `failed: executable`, and it's the only disagreement
in the run (clear-accept 6/7).

I chose not to loosen the check. A looser wording ("names the file or
code path") might flip pkg-14 to accept. But it could also let pkg-18's
"recover() safety net somewhere" or pkg-10's "Profile starship on Windows to find the slow parts" through. If I
loosened it, I'd re-run `--only pkg-14,pkg-10,pkg-17,pkg-18`, with the
three unbuildable packages as canaries, before another full run. Since
run 1 already scored 19/20 with every category matched, I accepted the
pkg-14 miss rather than risk a canary flipping. I made no other changes,
so nothing else in the run moved.
