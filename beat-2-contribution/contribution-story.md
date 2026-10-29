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
**Pull Request:** https://github.com/py-econometrics/pyfixest/pull/2014

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

**Unit 7: Installed the ship-gate, wrote a 3-check manifest for pyfixest, and ran it on my branch until it passed**

(2026-10-28) My manifest runs pyfixest's lint, type check, and the targeted `test_ritest.py` suite with stored R comparisons. The full `test-py` suite went into the excluded table with a dated reason: it already fails on master (one torch test) and rewrites a tracked file. The gate's first run on `feat/ritest-strata` passed all three checks, but pr-precheck rejected the branch because my local `master` was a week stale and it diffed 29 unrelated commits. After I fast-forwarded `master`, it rejected again, correctly, because my R cases and tolerance had drifted from `plan.md` with no record; I added a Deviations section and the third run passed. The gate didn't catch one thing: my description's AI line said I ran every command myself, when Claude Code ran them. I fixed that by hand, because no check reads a sentence for whether it's true.

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

pyfixest's `ritest()` runs randomization inference: it reshuffles the treatment many times to build the distribution of the estimate under "no effect", then asks how unusual the real estimate is. Many experiments are stratified (randomized separately inside blocks, like 25 of 50 people treated in each of 4 schools). For those, the reshuffle has to stay inside each block, or it describes a different experiment. `ritest()` had no way to say that: there was no `strata` option, unlike R's `ritest` and Stata's.

**Expected Behavior**

`fit.ritest("D", strata="block")` should permute the treatment within each stratum, so every draw keeps each stratum's treated count (25/25/25/25 in my example), as R's `ritest(..., strata=)` does. With `cluster=` too, whole clusters should move within their stratum.

**Current Behavior**

On master (`51774c7d`), `ritest(strata=...)` raises `TypeError: Feols.ritest() got an unexpected keyword argument 'strata'`. Each draw shuffles the treatment over the whole sample, which keeps the total treated count (100) but not each stratum's: per-stratum counts ranged 15 to 36, and only 0.1% of 1000 draws kept 25 in every stratum.

**Affected Components**

- `pyfixest/estimation/models/feols_.py`, `Feols.ritest()`: the public API and argument validation.
- `pyfixest/estimation/post_estimation/ritest.py`: `_assignment_units()` and the two places a draw happens, the slow path (`_get_ritest_stats_slow`) and the numba-compiled fast path (`_run_ri`).
- Tests: `tests/test_ritest.py`, and the R reference script `tests/r_test_comparisons.R` with its stored CSVs in `tests/data/`.

**Toolkit**

- Tool: my `CLAUDE.local.md` memory file for pyfixest (Unit 6), plus Claude Code reading the `ritest` code with me.
- What it produced: the call path `ritest()` -> slow/fast stats functions -> the draw step, and the test and R-reference files, without re-mapping the repo.
- Difficulties: the memory file was a week out of date. It still pointed at `_resample`, which #1965 removed on Oct 24, so its subsystem section was wrong about where the change goes.
- What changed: I traced the new code (`_assignment_units` and `rng.permutation(assignment)`) and wrote the affected components from it. The memory file's subsystem section needs the same update.

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

`Feols.ritest()` (`pyfixest/estimation/models/feols_.py:2061`) has no `strata` argument; passing one raises `TypeError: Feols.ritest() got an unexpected keyword argument 'strata'`. Since #1965 (merged 2026-10-24), both algorithms draw the null distribution the same way: `_assignment_units()` (`pyfixest/estimation/post_estimation/ritest.py:248`) collapses the treatment to its units of assignment (observations, or clusters when `cluster` is set), and each draw is `rng.permutation(assignment)[unit_codes]`, in the slow path (`_get_ritest_stats_slow`, `ritest.py:91`) and in the numba-compiled fast path (`_run_ri`, `ritest.py:237`). That permutation runs over all units at once, so it keeps the total treated count but not each stratum's. My reproduction on master `51774c7d` (4 strata x 50 obs, 25 treated each, 1000 draws) shows each stratum's treated count ranging from 15 to 36, with only 0.1% of draws keeping 25/25/25/25. For a stratified (blocked) experiment that is the null distribution of the wrong design. R's `ritest(..., strata=)` permutes within strata.

**Proposed Solution**

Add `strata: str | None = None` to `Feols.ritest()` and permute the treatment within each stratum, as R's `ritest` and Heß (2017, Stata Journal 17(3)) do.

- In `ritest()`: validate `strata` at the API boundary with `ValueError` (column not in the data; missing values; with `cluster`, a cluster that spans more than one stratum, because whole clusters are permuted within strata). Factorize it to integer codes, and pass them to both algorithms. Report `res["Strata"]` the way `res["Cluster"]` is reported.
- In `ritest.py`: add `_unit_strata()`, which maps the strata codes onto the assignment units from `_assignment_units()`, and a numba-compatible `_permute_within_strata(assignment, unit_strata, rng)`, which applies `rng.permutation` to each stratum's units. Call it at the two permutation sites (slow path and `_run_ri`) when strata are given.

Deliberately NOT changed:

- The no-strata path. It keeps calling `rng.permutation(assignment)` exactly as now, so the random stream, the existing results, the stored R comparisons, and the release contract are unchanged for every current call.
- The position of existing parameters. `strata` goes last in the signature, so positional calls like `fit.ritest("X1", "group_id", 100)` keep working.
- `_assignment_units()`'s signature and return value (it has its own test).
- Weights (`ritest()` still rejects them; that's #463) and the `cluster` logic.
- `pixi.lock`.

**Implementation Plan**

1. `pyfixest/estimation/post_estimation/ritest.py`: add `_unit_strata()` and `_permute_within_strata()`; thread an optional `unit_strata` through `_get_ritest_stats_slow`, `_get_ritest_stats_fast` and `_run_ri`, using it at lines 91 and 237. Keep the new helper numba-compatible (`_run_ri` is `nb.njit`-compiled).
2. `pyfixest/estimation/models/feols_.py`: add the `strata` parameter (last), its docstring and an example, the three `ValueError` checks, and pass the codes to both algorithms; add `res["Strata"]`.
3. `tests/r_test_comparisons.R`: add a strata block in the file's current `do.call` style (data with complete rows, `stratum = group_id %% 3`, so the `group_id` clusters nest in strata; formulas `Y~X1+f3` and `Y~X1+f3|f1`; resampvars `X1`, `f3`; with and without `cluster = "group_id"`; `reps = 10000`, `seed = 123`), writing `tests/data/ritest_strata_results.csv`. Regenerate with `pixi run --frozen -e py312-r Rscript tests/r_test_comparisons.R` and confirm the existing `ritest_results.csv` comes out unchanged. Add the new CSV to the `_update-test-data` task outputs in `pyproject.toml`.
4. `tests/test_ritest.py`: (a) `_permute_within_strata` keeps every stratum's treatment values (with and without clusters; whole clusters move together); (b) the three validation errors; (c) extended: slow and fast give the same statistics with the same seed; (d) extended: p-value, its SE and CI lower bound match R's stratified `ritest` within `rtol=atol=0.005`, the tolerance `test_vs_r` uses, because 10,000 R draws vs 4,000 pyfixest draws differ by Monte Carlo error. On master, (a) cannot import `_permute_within_strata` and (b) fails with `TypeError` for `strata`, so both fail without the change. (`store_ritest_statistics` stores only the statistics, not the draws, so per-draw counts are checked on the helper rather than through `ritest()`.)
5. `docs/changelog.qmd`: one entry linking #459. `docs/contributing.qmd`: mention the new CSV that `_update-test-data` produces.
6. Re-run my reproduction on the branch, expecting `ritest(strata=...)` to run and every draw to keep 25/25/25/25 (100% of 1000 draws), then the manifest checks (`lint`, `ty`, targeted `tests/test_ritest.py` in `py312-r`) and `test-py` by hand. Save the before/after output and the check results in `test_evidence.md`.

Not verified yet: that `rng.permutation` on each stratum's subset compiles under `nb.njit` inside `_run_ri` (master already calls `rng.permutation(assignment)` there, so I expect it to). If it doesn't, I'll draw a permutation of indices instead.

Open question for the maintainer (in the PR, not blocking): R's `ritest` also accepts `strata` together with `cluster`. I support that only when clusters nest in strata, and raise `ValueError` otherwise.

Deviations (recorded while building, 2026-10-28):

1. **R reference cases (step 3).** The plan named resampvars `X1` and `f3` with and without `cluster = "group_id"`. Since #1965, `ritest()` raises `ValueError` when the treatment varies within clusters, so permuting `X1` or `f3` by `group_id` is no longer allowed. The R block now mirrors master's own `test_vs_r` cases: `X1` and `D_unit` without clusters, `D_cluster` with `group_id` clusters, each for `Y~...+f3` and `Y~...+f3|f1` (6 cases, still `reps = 10000`, `seed = 123`, `stratum = group_id %% 3`).
2. **Tolerance (step 4d).** The plan said `rtol=atol=0.005` with 4,000 pyfixest draws, "as `test_vs_r` uses". Master's `test_vs_r` has since moved to 10,000 draws and a Monte Carlo tolerance (4 standard errors of the difference of two 10,000-draw estimates, plus 1e-3), so `test_ritest_strata_vs_r` uses that same rule and 10,000 draws.
3. **No `f3` resampling case and no `|f1+f2` formula** in the strata comparison, to keep the R run short (6 cases); master's non-strata cases still cover them.

**Toolkit**

- Tool: plan-check, grading `plan.md` and a short plan comment (`plan-comment.md`, not posted) against #459.
- What it produced: accept on the first run, all six required checks passing. It verified my cited lines (`ritest.py:91` and `:237`) and that #1965 had merged. It also flagged three weak spots: a "stored draw" test that can't be written through the public API (`store_ritest_statistics` keeps only statistics, not draws), no expected result for the repro re-run, and an untested assumption that `rng.permutation` compiles under numba.
- Difficulties: it said my plan-check skill had no `voice-guide.md`, so it skipped the voice pass, but the file is there as a symlink to my canonical guide. The tool seems not to follow the link. Its warning that my local `master` was stale turned out to matter a lot later, in the ship-gate.
- What changed: I dropped the impossible test, added the expected result (25/25/25/25 in 100% of draws) and the numba question to the plan. When the build differed from the plan, I recorded it under Deviations instead of quietly changing the plan.

---

## Testing Strategy

1. **Re-run the reproduction, before and after.** Same design (4 strata x 50 obs, 25 treated each, seed 459, 1000 draws). On master `51774c7d`: `ritest(strata='s')` raises `TypeError`, per-stratum treated counts range 15 to 36, and 0.1% of draws keep 25 in every stratum. On my branch: `ritest(strata='s')` runs, and every stratum has exactly 25 treated in 100% of draws.
2. **New tests that fail without the change.** 15 tests in `tests/test_ritest.py`: within-strata permutation keeps each stratum's values, with and without clusters, and whole clusters move together (2); the three `ValueError` cases: missing column, missing values, a cluster spanning strata (1); slow and fast algorithms give the same statistics from the same seed (6); and a comparison with R's stratified `ritest` on 6 cases with 10,000 draws, using the same Monte Carlo tolerance as the repo's `test_vs_r` (6). Run against unmodified master, the file fails with `ImportError: cannot import name '_permute_within_strata'`.
3. **External reference.** pyfixest's testing policy requires numerical changes to be checked against existing software, so I added a strata block to `tests/r_test_comparisons.R` and regenerated the data with `pixi run --frozen -e py312-r _update-test-data`. The existing `ritest_results.csv` came out byte-for-byte unchanged, which shows my R setup reproduces master's stored numbers.
4. **The repo's own checks (my ship manifest):** `pixi run --frozen lint` passed, `pixi run --frozen ty` passed (`All checks passed!`), and `pixi run --frozen -e py312-r pytest tests/test_ritest.py -q --no-cov` gave `105 passed` (90 on master, plus 15).
5. **Full Python suite, run by hand:** `pixi run --frozen test-py` gave `4979 passed, 1 failed` on the branch vs `4976 passed, 1 failed` on master. The one failure is the same torch-backend test on both (the optional `torch` extra isn't installed), and the +3 are my new non-extended tests.
6. **Unchanged behavior without strata.** The no-strata path still calls `rng.permutation(assignment)`, so master's 90 existing ritest tests, including the stored R comparisons, pass unchanged.

**Toolkit**

- Tool: my ship manifest (`ship-manifest.md`), run by the ship-gate, plus the repro script from Unit 6 re-pointed at the branch's draw step.
- What it produced: the before/after table above and `test_evidence.md` (lint, ty, 105 targeted tests, and the `test-py` comparison).
- Difficulties: `test-py` already fails on master (one torch test), takes 6 to 8 minutes, and rewrites a tracked file (`tests/texfiles/test.tex`) when it runs, so it can't sit in the must-pass table. My first `pixi run` also rewrote `pixi.lock` (a pixi version reformat), which pyfixest only accepts for intentional dependency changes.
- What changed: `test-py` went into the manifest's excluded table with a dated reason, and I ran it by hand on both master and the branch to compare. Every command now uses `pixi run --frozen`, and I restored the lock file.

---

## Implementation Notes

**2026-10-24**

First draft of the fix on my branch, on the code as of Oct 21: a `strata` argument with validation, a numba-compatible `_permute_within_strata` inside `_resample`, tests, and an R reference block. Not committed.

**2026-10-28**

Reproducing showed master had moved: #1965 (merged Oct 24) removed `_resample` and now permutes the observed assignment. My draft edited a function that no longer existed. I saved the draft on a local backup branch (`wip/ritest-strata-pre1965`) instead of discarding it, reset `feat/ritest-strata` to current master, and ported the idea onto the new code: `_unit_strata()` maps strata onto assignment units, and `_permute_within_strata()` permutes each stratum, called at the two draw sites.

**2026-10-28**

Decisions while building:
- I put `strata` last in the signature so `fit.ritest("X1", "group_id", 100)` still works.
- I left the no-strata path as `rng.permutation(assignment)` so every existing seed gives the same numbers.
- I passed `None` into the numba-compiled `_run_ri` when there are no strata, and checked numba compiles both versions.

The smoke test showed 25/25/25/25 in all 1000 draws, through the compiled helper.

**2026-10-28**

Wrong turn: calling `Rscript tests/r_test_comparisons.R` directly failed (`No module named 'pandas'`) because reticulate picked the system Python. The repo's own `_update-test-data` task sets `RETICULATE_PYTHON`, and it worked (about 6 minutes).

Also, my old draft's R cases (permuting `X1` by cluster) are now illegal on master, which rejects a treatment that varies within clusters. I switched to master's cases (`X1`, `D_unit`, `D_cluster`) and its Monte Carlo tolerance, and recorded both as plan deviations.

**2026-10-28**

Two commits:
- `test(ritest): add stratified R ritest reference data`
- `feat(ritest): permute the treatment within strata`

The ship-gate's first run rejected the branch: my local `master` was a week stale, so pr-precheck diffed 29 unrelated commits. I fast-forwarded `master` to upstream. The second run rejected it because the R cases and tolerance differed from the plan without a Deviations section. I added one, and the third run passed.

I also corrected my PR description's AI line, which wrongly said I had run every command myself; Claude Code ran them on my machine.

**Toolkit**

- Tool: Claude Code in my pyfixest clone, with `CLAUDE.local.md` and the repo's own `AGENTS.md`, plus git branches for safety.
- What it produced: the port onto post-#1965 code, two commits split by reviewer concern, and a backup branch holding my first draft.
- Difficulties: the upstream rewrite under my issue, a stale local `master`, a lock file that pixi rewrote on its own, and an R command that only works through the repo's task.
- What changed: I now fetch upstream before building, use `--frozen` for every pixi command, and run R through `_update-test-data`. My memory file should say all three.

---

## Pull Request

**PR Link:** https://github.com/py-econometrics/pyfixest/pull/2014 (opened 2026-10-28 as a draft, as pyfixest asks for AI-assisted work)

**PR Description:** Title "feat(ritest): support stratified randomization inference". It follows pyfixest's PR template (Summary, Verification):
- What it adds: `strata` in `ritest()`, permuting the treatment within each stratum the way R's `ritest(..., strata=)` does. With `cluster=`, whole clusters move within strata, and a cluster spanning strata raises `ValueError`. The no-strata path is unchanged, so existing results and seeds don't move.
- `Fixes #459`, and a note that the issue's `_resample` pointer was replaced by #1965.
- One open question for the maintainer: R also allows clusters that span strata.
- Verification: the before/after repro (per-stratum balance 0.1% -> 100% of draws), lint/ty/105 ritest tests passing, the new tests failing on master, the R comparison, and `test-py` matching master's one unrelated torch failure.
- An AI-use line saying I used Claude Code to port my earlier draft and write the tests and the R block, and that I reviewed and understand the change.

**Maintainer Feedback:**

- 2026-10-28: none yet (PR just opened as a draft).

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (Unit 7), running my 3-check manifest plus my pr-precheck before the PR opens, and my voice guard on the description.
- What it produced: 5 runs. Run 1 rejected: my local `master` was stale, so pr-precheck diffed 29 unrelated commits. Run 2 rejected: the R cases and tolerance had drifted from `plan.md` with no Deviations section. Run 3 passed. Run 4, through the hook when opening the PR, stopped on `ai-disclosure` because my AI line didn't say I understand the change. Run 5 passed, and the PR opened through the hook.
- Difficulties: the gate fingerprints the description, so every wording edit meant a fresh run (about 90 seconds of tests plus a pr-precheck call). It also can't tell whether a sentence is true: my first AI line said I ran every command myself, which passed every check.
- What changed: fast-forwarded `master` before gating, added Deviations to my plan, corrected the AI line by hand, and added a sentence saying I understand the change.

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
