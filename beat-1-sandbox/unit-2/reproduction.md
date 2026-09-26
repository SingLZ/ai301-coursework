# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

**GitHub username:** `SingLZ`

**Issue:** https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68 —
"Keyword search raises `ZeroDivisionError` when the index is empty"

---

## Claim comment

**Link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5850633965

**Text as posted**

> I'm new to this repo, and I'd like to take this one.
>
> I haven't run it yet — that's my next step, and I'll post what I get either
> way. Specifically:
>
> 1. Set the repo up per `docs/SETUP.md` on Python 3.11 and install with
>    `pip install -e ".[dev]"`, the repo's own install line.
> 2. Call `KeywordSearcher().index([])` directly to capture the raw traceback,
>    and run `pytest tests/unit/test_keyword_search.py -k test_empty_index` to
>    confirm the `xfail` marker (manifest H-01) is failing for the reason the
>    issue names and not for some other one.
> 3. Read `index()` against `search()`. `search()` returns `[]` when
>    `self.bm25` or `self.chunks` is falsy; `index()` has no equivalent guard
>    before it constructs `BM25Okapi` from the tokenized corpus. I want to
>    confirm from a traceback that this is where the division by zero comes
>    from, rather than assume it.
>
> I'll report back with the environment I ran and the output I actually saw —
> including a cannot-reproduce, if that is what I get.

**Reflection**

I ran `repro-check` in live mode on this draft before posting it, as a
claim-only package. Five checks came back `unclear` with "not yet applicable:
claim-only draft"; the two that could be graded — `claim-comment-specific` and
`ai-disclosure` — both passed, and the verdict was `accept`.

Two things came out of that run that I would not have caught by rereading the
draft myself.

The first is that my own `voice-guide.md` contradicted the assignment. Its
"Things I never post" list ended with "A claim on an issue I have not tried to
reproduce yet" — and this claim is exactly that, by design, because the claim
goes up before the reproduction. The rule I had written was aimed at drive-by
"+1, assign me" comments, but as worded it also banned the honest
promise-first claim this beat asks for. I rewrote that bullet to draw the line
where I actually meant it: what I never post is a claim that *asserts* the bug
reproduces before I have run it. Claiming first is fine; reporting a result I
do not have is not.

The second is about ordering. Two classmates had already claimed and
reproduced #68 before me. The house rule says a classmate's claim does not
block me and that I must not piggyback their repro, so I deliberately wrote
the claim without reading their evidence into mine — my claim names the
attempt I was about to make, in the terms I had gotten from the source, and
nothing in it depends on what they found.

---

## Repro comment

**Link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5850748432

**Text as posted**

> Reproduced on `main` at `2f4e82f`.
>
> **Environment**
>
> | | |
> |---|---|
> | Python | 3.11.7 (CPython) |
> | Platform | macOS 26.6.2, arm64 (Apple silicon) |
> | Shell | zsh |
> | `rank-bm25` | 0.2.2 (repo pin: `rank-bm25>=0.2.2`) |
> | `structlog` | 26.1.0 |
> | `numpy` | 2.4.6 |
> | `pytest` | 9.1.1 |
> | Repo commit | `2f4e82f52efbcfcc57d65b3fa5348672163ca088`, branch `main` |
>
> **Steps**
>
> From an empty directory:
>
> ```bash
> git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
> cd pathreview-ai301-fa26-s3
> python3 -m venv .venv
> .venv/bin/python -m pip install --upgrade pip setuptools wheel
> .venv/bin/pip install -e ".[dev]"
> ```
>
> That is the repo's own install line from the `setup` target in the
> `Makefile`. I stopped there rather than running `make setup` in full: this
> bug is reached without the database, so I did not run `docker compose up -d`,
> `alembic upgrade head`, or `scripts/seed_db.py`. Nothing below touches
> Postgres, Redis, or the API.
>
> Then, three runs.
>
> **1. The failing call, direct**
>
> ```bash
> .venv/bin/python -c "
> from rag.retriever.keyword_search import KeywordSearcher
> s = KeywordSearcher()
> s.index([])
> "
> ```
>
> ```
> Traceback (most recent call last):
>   File "<string>", line 4, in <module>
>   File ".../rag/retriever/keyword_search.py", line 25, in index
>     self.bm25 = BM25Okapi(tokenized_corpus)
>                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^
>   File ".../site-packages/rank_bm25.py", line 83, in __init__
>     super().__init__(corpus, tokenizer)
>   File ".../site-packages/rank_bm25.py", line 27, in __init__
>     nd = self._initialize(corpus)
>          ^^^^^^^^^^^^^^^^^^^^^^^^
>   File ".../site-packages/rank_bm25.py", line 52, in _initialize
>     self.avgdl = num_doc / self.corpus_size
>                  ~~~~~~~~^~~~~~~~~~~~~~~~~~
> ZeroDivisionError: division by zero
> ```
>
> **2. The covering test**
>
> ```bash
> .venv/bin/python -m pytest "tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index" -rx
> ```
>
> ```
> collected 1 item
>
> tests/unit/test_keyword_search.py x                                      [100%]
>
> =========================== short test summary info ============================
> XFAIL tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index - issue #68 (manifest H-01): BM25 keyword search raises ZeroDivisionError on an empty index
> ============================== 1 xfailed in 0.83s ==============================
> ```
>
> The marker is `strict=True`, so `XFAIL` here means the test still fails, and
> it fails for the reason the marker names rather than passing unexpectedly.
>
> **3. Control — the same call with one chunk, and `search()` before `index()`**
>
> ```bash
> .venv/bin/python -c "
> from rag.retriever.keyword_search import KeywordSearcher
> s = KeywordSearcher()
> s.index([{'id': 1, 'text': 'python programming'}])
> print('index([1 chunk]) -> OK, bm25 =', type(s.bm25).__name__)
> print('search(\"python\") ->', s.search('python'))
> u = KeywordSearcher()
> print('search() before index() ->', u.search('python'))
> "
> ```
>
> ```
> index([1 chunk]) -> OK, bm25 = BM25Okapi
> search("python") -> [{'id': 1, 'text': 'python programming', 'bm25_score': -0.2746530721670274}]
> search() before index() -> []
> ```
>
> And the whole file, for regression context:
>
> ```bash
> .venv/bin/python -m pytest tests/unit/test_keyword_search.py -q
> ```
>
> ```
> ........x........                                                        [100%]
> 16 passed, 1 xfailed in 0.18s
> ```
>
> **What this shows**
>
> The empty list is the trigger and nothing else is. One chunk indexes fine,
> and `search()` on a searcher that was never indexed already returns `[]` — so
> the empty case is handled on the read path and unhandled on the write path.
>
> The traceback puts the division in `rank_bm25.py` line 52,
> `self.avgdl = num_doc / self.corpus_size`, reached from `keyword_search.py`
> line 25. `index()` builds a `BM25Okapi` unconditionally:
>
> ```python
> self.chunks = chunks
> tokenized_corpus = [self._tokenize(chunk["text"]) for chunk in chunks]
> self.bm25 = BM25Okapi(tokenized_corpus)
> ```
>
> while `search()` opens with the guard `index()` lacks:
>
> ```python
> if not self.bm25 or not self.chunks:
>     logger.warning("keyword_search_empty_index")
>     return []
> ```
>
> **Verdict: reproduced**, on the version and platform above. I have not tested
> any other Python version or OS, and I have not tested a fix — the asymmetry
> between the two methods is what the traceback and the control run support,
> and that is as far as this report goes.

**Reflection**

`repro-check` graded the full package before this went out and returned
`accept` with all seven required checks passing.

The check I had to work hardest to satisfy was `steps-rerunnable`, and the
reason is the one place my run departs from the repo's documented setup. The
repo's `make setup` brings up Docker, runs `alembic upgrade head`, and seeds
the database. I ran only the `pip install -e ".[dev]"` line out of that target,
because this bug lives in a pure unit-level module and never touches Postgres
or Redis. My own check treats an inferred step as fatal — "Fail if any step
requires an action the reader has to infer" — so the honest move was not to
quietly omit the difference but to name it in the report, with the reason a
reader can verify for themselves. A stranger following my steps gets my
traceback; a stranger following `make setup` gets it too, just slower.

The control run is the part I would have skipped a week ago. On its own the
traceback proves an exception happened; it does not prove the empty list is
what caused it. Indexing one chunk successfully, and showing that `search()`
already returns `[]` on an unindexed searcher, is what turns the traceback
from an anecdote into evidence about the specific trigger the issue names —
and it is what makes the report mine rather than a restatement of the two
repro comments already on the thread.

---

## Eval iterations

**Run history**

One run.

1. Full run with `--save-run eval-run.txt`, grader model `sonnet` (pinned),
   20 scored packages: **agreement: 19/20 scored items (bar: 18/20: PASS)**,
   with `categories: clear-accept 7/8  disclosure 1/1  no-evidence 4/4
   unfollowable-comms 3/3  wrong-target 4/4`. Every category matched, so the
   category floor held. The single disagreement was `pkg-05`, noted by the
   harness as `failed: steps-rerunnable`. That is the run committed as
   `eval-run.txt`, written at `2026-09-24T02:07:33Z`.

I did not run a revise loop after it, and I did not revise the rubric between
writing it and running it. The components I took into that run — `rubric.md`
with the worksheet changes from the activity folded in, plus
`references/evidence-guide.md` — are byte-for-byte the ones in this folder:
the header of `eval-run.txt` fingerprints `rubric.md` as
`sha256:7b99f4b2418f1eb2` and `evidence-guide.md` as `sha256:00a6262f007c6670`,
and both files still hash to those values. Nothing was loosened after the fact,
so there was no canary to run.

I want to be straight about what that means rather than dress it up as a
disciplined choice: it passed on the first full run, and the one disagreement
left is a call I decided to keep rather than a bug I ran out of time to fix.
The reasoning for keeping it is in Trade-offs below.

**Package analysis**

`pkg-05` (conda/conda#16543, "EnvironmentSectionNotValid message breaking json
output").

- My rubric's verdict: **reject**, on `steps-rerunnable`. Harness note:
  `failed: steps-rerunnable`.
- Gold label: **accept**.
- Why my rubric read it that way. Every other check on this package is clean.
  The environment record is complete ("conda 26.7.0 (miniforge3), Python
  3.12.7, macOS 15.5 (osx-arm64), libmamba solver") and matches the reporter's
  `conda info`. The artifacts are verbatim and there are two of them — the
  polluted stdout, and a second run piping it into `python3 -m json.tool` to
  show it fails to parse. The failure shown is the failure the issue describes.
  The claim names the version and a concrete next step with no date. The
  conda policy explicitly welcomes generative AI, so `ai-disclosure` passes
  with nothing to do. It came down to one clause of one check.

  That clause is "config and input content given inline". The report's steps
  begin: *"wrote a minimal `env.yml` containing a valid `dependencies:` list
  plus a `category:` section (the section conda does not recognize)"*. The
  `env.yml` is **described, not pasted**. My check reads an asset the reader
  must construct as an asset they have to infer, and my verdict rule counts a
  single required failure as a reject — so it rejected.

  Read against the gold label, the description is reconstructible in a way I
  did not credit. The bug is a stream-routing bug: the warning goes to stdout
  instead of stderr. Which packages sit under `dependencies:` is irrelevant to
  it — any valid list plus an unrecognized `category:` key triggers the same
  message. So a stranger holding only this package *can* rebuild the input and
  will hit the same failure, which is the thing `steps-rerunnable` is actually
  supposed to measure. My check measured whether the file was pasted, which is
  a proxy for that, and on this package the proxy and the thing came apart.

**Check rationale**

The check, quoted as it stands in the uploaded `tools/repro-check/rubric.md`:

> | steps-rerunnable | The steps section read as a stranger holding only this
> package would follow it, end to end, from their own machine. | The steps are
> an ordered sequence from a stated starting point (empty directory, fresh
> install, the issue's own file or playground link) in which every action is
> concrete enough to repeat — exact commands, exact keystrokes or UI actions,
> config and input content given inline — and every asset needed is in the
> package. Fail if any step requires an action the reader has to infer ("ran
> the usual setup", "our pre-commit hook") or an asset they cannot obtain
> (private repo, unshared config). | required |

The reasoning behind the wording I kept. The check's subject line is
deliberately not "are the steps complete?" but "the steps section read as a
stranger holding only this package would follow it" — the test is a person
attempting the run, not a checklist of sections present. Everything after it
is meant to serve that test.

The two failure examples name the two shapes that actually stop a stranger
cold, and they are different problems. `"ran the usual setup"` is an action
the reader cannot perform because they do not know what it refers to. A
private repo or unshared config is an asset the reader cannot obtain at any
price. Both end the attempt; neither is a matter of degree.

"Config and input content given inline" sits in the pass condition rather than
the fail examples, and after `pkg-05` I read that placement as load-bearing
rather than incidental. It describes what good looks like, and the fail
examples are what actually sinks a package. A described-but-unpasted config is
neither of the two fatal shapes: the reader can obtain it, because they can
write it. My grader collapsed that distinction and treated the pass
condition's clause as if it were a third fail example.

**Trade-offs**

I kept the check as written and accepted the `pkg-05` miss rather than adding
"or reconstructible from its description" to the pass condition.

What that costs me is exactly one kind of package: a genuinely complete report
whose input file is described precisely enough to rebuild. `pkg-05` is that
package, and the miss is a false reject — I hold a good report instead of
posting it.

What loosening would have cost is worse for what I am using this tool for.
"Reconstructible from its description" has no floor. Every unpasted config is
reconstructible by *someone*, and the judgment of how much description is
enough is the judgment I built the check to take away from me. The failure
mode I am actually guarding against is my own: shipping a report that reads as
complete to me, because I still have the file open in another window, and is
not followable by anyone else. A check that errs toward "paste it" costs me
thirty seconds per report. A check that errs toward "they can probably work it
out" costs me a thread I cannot get back.

The direction of the error matters too. A false reject sends me back to paste
a config I already have. A false accept sends an unfollowable report upstream
under my name. At 19/20 with every category matched, spending my one
disagreement on the harmless direction is the trade I would make again.

I would revisit this if the miss generalized — if a second package failed
`steps-rerunnable` for the same reason, that would be the check misreading a
whole class of reports rather than making one arguable call, and the fix would
belong in the pass condition. On this eval set it did not: `steps-rerunnable`
was the deciding failure on the `unfollowable-comms` packages it was written
for, and all 3 of those matched gold.
