# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

**Verdict output**

Live-mode run of the installed skill on three candidates (#73, #68, #62). The chosen
issue, #68, is ranked first and its verdict object is first in the closing JSON block.

````
All three are in the scoped repo (`codepath/pathreview-ai301-fa26-s3`). Repo-level evidence gathered once, applied to each candidate independently.

**Shared repo facts** (capture date 2026-09-17): `archived: false`; last 5 default-branch commits all authored by `Aburke225` (human, non-bot), newest 2026-09-16; no releases ever published, `pushed_at: 2026-09-16`; `docs/CONTRIBUTING.md` + `.github/PULL_REQUEST_TEMPLATE.md` both silent on AI, no AI-policy file; repo has **0 pull requests total**; maintainer response sample — #52 and #43 each got a COLLABORATOR reply 6 days after opening.

## Ranked read-out

**All three accept.** Every required check passes on each, and each also passes all three preferred checks, so the fit profile breaks the tie.

**1. #68 — `ZeroDivisionError` when the keyword index is empty** — best fit. A Python bug fix with a reproduction you already have: the covering test exists, marked `@pytest.mark.xfail` (manifest H-01), so `pytest` alone shows you red→green. Names both files to touch (`rag/retriever/keyword_search.py`, `tests/unit/test_keyword_search.py`), and needs no container, database, or build to see the bug — your top-priority category with the lightest setup of the three.

**2. #73 — README and `.env.example` disagree about the LLM API key** — documentation-only (your second preference), names both files, zero setup to verify. Ranked below #68 only because you put "small bug fix with clear reproduction steps" ahead of docs.

**3. #62 — health check references non-existent `settings.redis_host`** — a genuine Python bug fix with explicit repro steps, but the stated reproduction is `GET /health` **with Redis running**, so seeing the failure means bringing up Redis and the FastAPI app. Your profile ranks container/database setup lower, which drops it to third despite the good write-up.

One note on grading tension, not a verdict change: `maintainer-responsive` passes on a sample of two, and both sampled comments were the maintainer *closing* issues as invalid rather than engaging a contributor. The rubric's condition is met as written; if you want that distinction to count, it belongs in the rubric.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "All 5 latest main commits by Aburke225 (non-bot); newest 2026-09-16, 1 day before capture date"},
      {"name": "project-in-use", "grade": "pass", "evidence": "archived: false; releases array empty (never released), so fallback applies: pushed_at 2026-09-16, within 90 days"},
      {"name": "scope-one-change", "grade": "pass", "evidence": "One bounded fix: guard the empty corpus in index() and drop the @pytest.mark.xfail on the covering test; no disqualifier present (bug report, good-first-issue labeled, no closed attempts)"},
      {"name": "not-already-taken", "grade": "pass", "evidence": "assignees: []; timeline shows only 4 label events and no cross-references; repo has 0 PRs total; 0 comments"},
      {"name": "ai-contributions-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no statement on AI, generative AI, or AI-generated code; silence passes"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Sampled #52 and #43: both got a COLLABORATOR comment 6 days after opening (2026-09-10 to 2026-09-16)"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels include 'good first issue' and 'tier-1'; body names rag/retriever/keyword_search.py and tests/unit/test_keyword_search.py"},
      {"name": "issue-still-warm", "grade": "pass", "evidence": "Opened 2026-09-10T21:41:05Z, 7 days before the capture date"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "All 5 latest main commits by Aburke225 (non-bot); newest 2026-09-16, 1 day before capture date"},
      {"name": "project-in-use", "grade": "pass", "evidence": "archived: false; releases array empty (never released), so fallback applies: pushed_at 2026-09-16, within 90 days"},
      {"name": "scope-one-change", "grade": "pass", "evidence": "'Make the two files agree' — one coherent docs task across README.md and .env.example, both in the same area; labeled bug/docs so the unendorsed-feature disqualifier is exempt anyway"},
      {"name": "not-already-taken", "grade": "pass", "evidence": "assignees: []; timeline shows only 4 label events and no cross-references; repo has 0 PRs total; 0 comments"},
      {"name": "ai-contributions-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no statement on AI, generative AI, or AI-generated code; silence passes"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Sampled #52 and #43: both got a COLLABORATOR comment 6 days after opening (2026-09-10 to 2026-09-16)"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels include 'good first issue', 'docs', 'tier-1'; body lists relevant files README.md and .env.example"},
      {"name": "issue-still-warm", "grade": "pass", "evidence": "Opened 2026-09-16T21:51:36Z, 1 day before the capture date"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "All 5 latest main commits by Aburke225 (non-bot); newest 2026-09-16, 1 day before capture date"},
      {"name": "project-in-use", "grade": "pass", "evidence": "archived: false; releases array empty (never released), so fallback applies: pushed_at 2026-09-16, within 90 days"},
      {"name": "scope-one-change", "grade": "pass", "evidence": "One bounded fix: point the Redis probe in api/routes/health.py at the existing settings.redis_url; no disqualifier present (bug report with repro steps, no closed attempts, no open design question)"},
      {"name": "not-already-taken", "grade": "pass", "evidence": "assignees: []; timeline shows only 4 label events and no cross-references; repo has 0 PRs total; 0 comments"},
      {"name": "ai-contributions-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no statement on AI, generative AI, or AI-generated code; silence passes"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Sampled #52 and #43: both got a COLLABORATOR comment 6 days after opening (2026-09-10 to 2026-09-16)"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels include 'good first issue', 'api', 'tier-1'; body names api/routes/health.py and core/config.py plus explicit steps to reproduce"},
      {"name": "issue-still-warm", "grade": "pass", "evidence": "Opened 2026-09-10T21:40:55Z, 7 days before the capture date"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

**Run history**

Four runs, in order:

1. `--only issue-01,issue-13,issue-15,issue-20` (partial canary on the four calls my
   first-draft rubric was least sure of): **3/4**. issue-01 was the disagreement.
2. `--only issue-01,issue-05,issue-15,issue-19` (partial, after rewriting the scope
   disqualifiers): **4/4**.
3. Full run: **agreement: 20/20 scored items  (bar: 18/20: PASS)**.
4. Full run, `--save-run eval-run.txt`, after one editorial fix to the rubric (the scope
   row said "the five disqualifiers" over a list of six; re-run so the committed
   transcript fingerprints the rubric I actually uploaded): **agreement: 20/20 scored
   items  (bar: 18/20: PASS)**, with `categories: claimed 4/4  clear-accept 8/8
   dead-repo 3/3  policy 1/1  scope 4/4`. That is the run committed as `eval-run.txt`.

**Issue analysis**

`issue-01` (conda/conda#16475, "Add permanent docs for installing PyPI packages with
`conda install`").

- My rubric's first decision: **reject**, on `scope-one-change`. The harness note read
  `failed: scope-one-change, maintainer-responsive (preferred)`.
- Gold label: **accept**.
- Why my rubric read it that way: the note names the check but not the clause, so I read
  the bundle back against my first draft. Two clauses of that draft did it. The pass
  condition asked for "one bounded change that a single pull request could deliver",
  and the draft's closing note told the grader in as many words that a long issue is
  suspect: "a one-line bug report from a collaborator can be a perfectly bounded first
  issue, and a long, carefully written body can still be asking for a month of work."
  issue-01 is exactly the shape that wording punishes. Its body is a plan with five
  headings — "### Add a new task page", "### Update `manage-pkgs.rst`",
  "### Update `pip-interoperability.rst`", "### Update `new-features.md`",
  "### Consider a global `troubleshooting.rst` entry" — and the new page alone is
  specified with seven bullets ("what the workflow does", "required setup: min conda
  version, adding `conda-pypi` channel, channel priority", "troubleshooting or
  limitations", and so on). Read at a glance that is five files and a month of work;
  read honestly it is one coherent docs task, every edit in the same docs tree, that one
  PR can deliver.
- I had written the rule to guard against the trap in the evidence guide — "Grade the
  size of the work being asked for, not the polish of the writeup" — and ended up
  grading polish anyway, just with the sign flipped: I punished detail instead of
  rewarding it. The rest of issue-01's evidence is clean (`archived: no`, `last push to
  any branch: 2026-08-04`, `this issue: assignees: none; linked PRs: none`, and a
  contribution policy that says "generative AI tools welcome"), so scope was the only
  thing standing between this issue and an accept.

**Check rationale**

The check I rewrote, quoted as it now stands in the uploaded `tools/issue-select/rubric.md`:

> | scope-one-change | The issue title and body, plus the whole comment thread | The issue asks for one bounded change that a single pull request could deliver, and none of the disqualifiers in "Scope disqualifiers" below is present | required |

and the part of "Scope disqualifiers" that replaced the file-counting rule:

> **Detail, length, or several files.** One coherent task is one change even when it is
> spelled out at length or spans several files in the same area (a set of documentation
> pages, four package files). Likewise, a diagnosis or a list of suggested optimizations
> inside a bug report describes how one defect might be fixed; it is not a demand for
> several separate pieces of work.

The reasoning: a file count is a proxy for effort, and it is a bad one. What actually
makes an issue too big for a newcomer is not how many files move but whether the issue
has a single done state and whether somebody has to make design decisions to reach it.
So the check now names the six conditions that genuinely destroy a single done state —
umbrella/tracking list, standing invitation, core internals, unendorsed feature,
repeatedly attempted and dropped, and support question — and says in as many words
that length and file spread are not among them.

**Trade-offs**

The rewrite gives up the ability to catch a genuinely sprawling issue that is *written*
as one coherent task. Nothing in the check measures effort any more, so a "rewrite the
docs site" issue with a tidy one-line body would pass scope and would have to be caught,
if at all, by another check or by me. I accept that miss: the version that measured
effort cost me a real accept (`issue-01`), and file-spread is the wrong proxy for
effort in the first place.

The canary I re-ran to see what else the loosening moved was
`--only issue-01,issue-05,issue-15,issue-19`, and the answer was nothing I did not want:
4/4. issue-01 flipped reject → accept (gold accept). issue-05 and issue-15 stayed
reject, because they fail on disqualifiers the rewrite did not touch — issue-05 on the
standing invitation ("PRs are welcome both big and small (better to start small) that
add type annotations to parts of the codebase"), issue-15 on repeated abandonment
(`linked PRs: zulip/zulip#20840 (closed); zulip/zulip#23123 (closed)`, plus a thread of
`@zulipbot claim` comments each followed two weeks later by "You have been unassigned
from this issue because you have not made any updates for over 14 days"). issue-19 was
in that canary as a new probe rather than a re-run — I had not graded it before the
rewrite — because it is the case the old "Rework, not repair" clause was written to
sink ("There are two potential causes which should be fixed" plus "We should use
multi-processing"); under the new wording it accepts, and gold says accept. The full run
afterwards confirmed all of it at 20/20 with every category matched.

---

## Selection rationale

**Selection rationale**

1. **Fit and time.** #68 is a Python bug — `ZeroDivisionError` in keyword search when the
   index is empty — and Python is the language I actually work in. It names both files
   involved (`rag/retriever/keyword_search.py` and its unit test), and the covering test
   already exists marked `xfail`, so reproducing it is one `pytest` run rather than an
   afternoon of environment setup. That matters: the two other candidates were a
   docs-only fix (#73, less code to learn from) and a health-check bug (#62) whose
   reproduction needs Redis and the API running. With one unit left before I need a PR,
   the one I can see fail locally in a minute is the right one.

2. **What the verdict caught, and what I weighed myself.** The skill verified the things
   I would have taken on trust: that the repo is alive (five recent commits by a human,
   pushed the day before), that nobody holds the issue (no assignee, no linked PR, the
   repo has zero PRs total), and that no contribution policy blocks AI-assisted work.
   What it could not weigh is that an `xfail`-marked test is a gift — it tells me exactly
   what "done" looks like before I write a line — and that a `ZeroDivisionError` on an
   empty collection is a bug shape I have hit in my own code, so I already know roughly
   what the fix looks like. The rubric also flagged a tension I am carrying forward: it
   passes `maintainer-responsive` on a sample of two replies, and both of those replies
   were a maintainer closing an issue rather than helping a contributor.

3. **Claiming it.** I expect the claim itself to be low-friction — the issue is
   unassigned, and the Path Review house rule says classmates' claim comments do not
   block me, so the worst case is that someone else opens a PR for the same bug and both
   of ours count. The real risk is later: this is a shared classroom repo where the
   maintainer's only visible behaviour so far is closing issues, so I should not count on
   a reply to my claim comment and should keep the PR self-explanatory.
