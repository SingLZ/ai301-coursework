# Contribution: ritest: Add support for stratified sampling

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Sing Zheng
**GitHub Username:** SingLZ
**Issue:** https://github.com/py-econometrics/pyfixest/issues/459
**Pull Request:** not opened yet

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Turned my Unit 1 grader into issue-scout and pointed it at C++/quant issues across all of GitHub**

(2026-10-14) I installed the scout over my Unit 1 skill, rewrote my fit profile for C++ quant finance, and wrote two queries (QuantLib "help wanted" issues and a C++ keyword search); testing them by hand showed that OR terms inside one pair of quotes return 0 results and that `quant` and `options` pulled in ML quantization and settings menus, so I changed the keywords. My first scan rejected all five QuantLib bugs it graded because each already had an open PR, which the search results don't show, so I added `-linked:pr` to both queries and it removed 4 of the 5 when I re-ran it by hand. Grading my own picks, the scout called an Arrow PR that a bot had closed three days earlier "abandoned", so I patched my rubric's not-already-taken check to treat a PR auto-closed in the last 30 days as a live claim.

**Unit 6: Added an Upstream section to my voice guide, wired the voice guard as a hook, and wrote a CLAUDE.local.md for pyfixest**

(2026-10-21) I merged my three voice-guide copies into one, added rules for claims, questions and pings, installed the guard as a PreToolUse hook (the wiring test stopped `gh issue comment --help` as expected), and wrote a 33-line `CLAUDE.local.md` mapping `ritest()` -> `_resample` -> the tests, kept out of my diff with `.git/info/exclude`. Running the guard on the claim I had already posted, it stopped on my Unit 2 rule "No dates, no guarantees" because of "I'm aiming to open a PR within about two weeks". I decided the rule, not the claim, was wrong: the course asks claims for a realistic timeframe, and a hedged estimate isn't a guarantee, so I rewrote it as "Estimates, never guarantees" and made the same change in repro-check's claim-comment-specific check, after which my posted claim passed both. Writing the memory file, I found my local clone pointed at upstream instead of my fork, so I re-pointed the remotes, and I noted that `_resample` is numba-compiled, so my change has to stay numba-compatible.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I wanted my first contribution to be C++ quant finance, so I rewrote my fit profile around option pricing, order books and backtesting, and pointed my scout at QuantLib's "help wanted" issues plus a C++ keyword search. The first scan graded five QuantLib pricing bugs and rejected all five, because each already had an open PR from someone else. When I went through every issue on the Issue Selection site, the only finance projects were openalgo, where all 16 issues were already claimed (mostly by classmates), and pyfixest, a Python econometrics library. I graded pyfixest #459 directly and the scout accepted it: active maintainers who replied within hours, a `good first issue` label, and a body that names the function and test files. After this week's scope patch it rates it **straightforward**.

My scout also flagged that #459 is a poor fit for my C++ quant profile, and I chose it anyway on purpose. It's Python, but the work is randomization inference (resampling a treatment to build a null distribution and a p-value), which is the same kind of statistics quant research runs on, and I'd rather ship a real PR in a well-run numerical library than wait for a C++ issue nobody has taken. I hope to learn how a project like this proves numerical code is right, since every change here has to be checked against R's `ritest`.

Claim comment: https://github.com/py-econometrics/pyfixest/issues/459#issuecomment-6183148490 (the maintainer replied "Sounds great! I'll assign you to the issue").

An honest read of the scout: its rubric was the reliable part, and my search strategy was the weak part. The rubric correctly rejected all five QuantLib issues and both openalgo picks for being taken, which the Issue Selection site didn't show. But my "fewest comments first" ranking pointed it straight at freshly filed issues that people had already jumped on, so I added `-linked:pr` to my queries. The rubric also had two real gaps on wild issues. It counted an Arrow PR that a bot had auto-closed three days earlier as "abandoned", and after I claimed #459 it rejected my own issue because it couldn't tell my claim from a stranger's. I patched both.

**Toolkit**

- Tool: issue-scout (my search strategy and my rubric). I ran one scan over C++/QuantLib queries, then graded my Issue Selection picks directly (arrow #30923, openalgo #1828 and #1831, pyfixest #459 and #463), and re-graded them after each rubric patch.
- What it produced: the scan rejected all 5 QuantLib issues on not-already-taken. On #459 the verdicts went accept (10-14), then reject (10-21, on my own claim), then accept again after the fix, with all 8 checks passing and a difficulty note of straightforward.
- Difficulties: search results don't show linked PRs, so my ranking surfaced already-taken issues. The rubric treated a bot-closed PR as abandoned and my own claim as a stranger's. The old `scope-one-change` check would have rejected any issue with an open design edge.
- What changed: added `-linked:pr` to both queries (4 of 5 taken issues dropped out). Patched not-already-taken twice: a PR auto-closed in the last 30 days counts as live, and claims by SingLZ never block. Replaced `scope-one-change` with the staff `scope-feasible` row plus a difficulty note, keeping umbrella / standing-invitation / support-question as the only rejections and moving the other four disqualifiers into "ambitious".

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

- macOS 26.6.2 on arm64 (Apple Silicon), zsh.
- pyfixest master at commit `51774c7d` (2026-10-28), checked out as a clean `git worktree` of `upstream/master` with no local changes. It reports `pyfixest 0.60.0` because master hasn't been released past v0.60.0.
- pixi 0.83.0 (pyfixest's package manager; the repo needs >= 0.71.0), environment `py312`: Python 3.12.12, numpy 2.4.2. Setup: fork and clone pyfixest, `brew install pixi`, then any `pixi run` builds the environment, including the Rust extension.

Challenges: my first `pixi run` rewrote `pixi.lock`, and the repo's rules say to change it only for intentional dependency changes, so I now run everything with `pixi run --frozen` and will keep the lock file out of my PR. My working branch also already had my in-progress changes on it, so I reproduced on a separate clean worktree of master instead of stashing them. The biggest surprise was that master had moved: #1965 (merged 2026-10-24) removed `_resample`, the function the issue and my claim pointed at, and replaced it with a permutation of the observed assignment, so I reproduced against the new code.

**Steps to Reproduce**

1. Check out pyfixest master at `51774c7d` (`git worktree add --detach ~/pyfixest-master upstream/master`).
2. Run a short script (`repro_459.py`) with `pixi run --frozen -e py312 python -I repro_459.py`. It builds a stratified experiment with 4 strata x 50 observations and exactly 25 treated per stratum (seed 459), fits `feols("Y ~ D | s")`, and calls `fit.ritest(resampvar="D", strata="s")`.
3. It then makes 1000 draws using the exact step `ritest()`'s slow and fast paths use on master, `rng.permutation(assignment)[unit_codes]` from `_assignment_units`, and counts treated units per stratum in each draw. A stratified test should keep 25 treated in every stratum (R's `ritest(..., strata=)` permutes within strata).
4. Observed: `ritest(strata=...)` raises `TypeError`, and per-stratum treated counts swing from 15 to 36, so only 0.1% of draws keep 25 in every stratum.

**Reproduction Evidence**

- **Commit showing reproduction:** none (the script lives outside the repo and ran on unmodified upstream commit `51774c7d`).
- **Screenshots/logs:** output excerpt:

  ```
  ritest parameters: ['resampvar', 'cluster', 'reps', 'type', 'rng', 'choose_algorithm', 'store_ritest_statistics', 'level']
  1. ritest(strata='s') -> TypeError: Feols.ritest() got an unexpected keyword argument 'strata'
  2. total treated per draw: min 100 max 100
     observed treated per stratum: [25, 25, 25, 25]
     stratum 0: treated count over 1000 draws min=16 max=36 mean=25.0; draws keeping exactly 25: 12.2%
     stratum 1: treated count over 1000 draws min=15 max=34 mean=25.0; draws keeping exactly 25: 12.4%
     stratum 2: treated count over 1000 draws min=15 max=34 mean=25.2; draws keeping exactly 25: 14.5%
     stratum 3: treated count over 1000 draws min=17 max=35 mean=24.8; draws keeping exactly 25: 13.4%
     draws keeping 25 in ALL strata: 0.1%
  ```

- **My findings:** The gap is real on today's code. The public API rejects `strata`, and the permutation keeps the total treated count fixed (100 in every draw) but not the per-stratum counts, so `ritest()` builds the null distribution of a completely randomized design, not a stratified one. Because #1965 removed `_resample`, the plan in my claim ("resample within strata in `_resample`") is out of date. From reading the code (not tested yet), I think the change now belongs in `_assignment_units` and the two `rng.permutation(assignment)` call sites. Not checked yet: R's `ritest` output on the same data, and the `cluster` path, which uses the same permutation over cluster units.

**Toolkit**

- Tool: repro-check, grading my claim (`claim-draft.md`, the text I posted on Oct 15) and my repro report (`repro-draft.md`) together, now with the wild scope instead of the sandbox one.
- What it produced: the first run rejected the pair, failing steps-rerunnable because my report said "Save `repro_459.py` (below)" but never included the script, and flagged that I stated the fix location as fact. The second run accepted with all 7 checks passing, and it verified the commit, PR #1965, and both permutation call sites on its own.
- Difficulties: the issue's 2024 file pointer no longer existed, so I had to work out what "reproducing a feature request" means on code that had changed under the issue. It also confirmed that my unposted claim revision shouldn't be posted, since its Friday date had passed and its `_resample` question was outdated.
- What changed: I embedded the full script in a `<details>` block, reworded the fix location as "From reading the code (not tested yet), I think…", and added the note above that my claimed plan changes because of #1965.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
