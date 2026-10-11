# Rubric: is this pull request ready to submit?

Seven required checks and one preferred. Each required check belongs to
one failure family:

- Silent drift: `diff-matches-plan`, `description-matches-diff`.
- Not tested: `repro-rerun-decisive`, `repo-checks-run`.
- Unreviewable: `diff-free-of-debris`.
- Standards wall: `template-asks-met`, `ai-disclosure`.

The preferred check reads the commit list and never changes the
verdict.

Assumptions the checks run on:

- Every package graded here is AI-assisted work.
- The plan, including its deviation notes and stated deferrals, is the
  contract. A shortfall the plan or description discloses is honest
  and passes. Only an undisclosed gap fails.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | Every file and hunk in the diff (live: `git diff main...HEAD`), read against the plan's Scope, Files, and Approach, its not-in-scope line, and its deviation or deferral notes (live: `plan.md` including `## Deviations`). | Both directions hold. (1) Every hunk that changes behavior or code is a change the plan names, a test or doc for that change, a repo-required file the plan or template names (a changelog or whatsnew entry), or is recorded in a deviation note. (2) Every change the plan commits to appears in the diff, or the plan or description says it was deferred or dropped, and why. Fail if the diff adds a function rewrite, a refactor, a rename pass, a new option or flag, a dependency bump, or a hunk in a file the plan never names, with no deviation note. Fail too if a planned change is missing with no note. An explanatory code comment on the planned change is not drift. | required |
| description-matches-diff | Each factual claim in the PR title and description (what changed, which files, "exactly as planned", "no other changes", "docs updated", "tests added", "CHANGELOG entry added"), checked against the diff. | Every claim the description makes is visible in the diff, and the description does not call the change "only", "exactly as planned", or "no other changes" while the diff does more. Fail if it claims something the diff does not contain (docs or tests it says it added but did not), or claims fidelity the diff contradicts. Only claims about what this PR changes are checked against the diff. Context is not a diff claim and is never failed here: the old behavior, a "before" transcript, the motivation, a thread discussion, or a deferral note. Silence about a minor test file is not a false claim. | required |
| repro-rerun-decisive | The test evidence section and the description's before/after (live: `test_evidence.md` and `pr_draft.md`), read against the plan's Test plan and the repro evidence's failing input and Actual result. | The evidence re-runs the failing case the plan's test plan names (the repro's own input, not a control or a different path) on the branch. It shows an observable result that differs from the repro's Actual result, and it matches the plan's expected-after: output, exit code, a printed value, or a named test asserting that exact case passing. A before line may come from the repro evidence. Every distinct failing scenario the test plan names must be shown, unless the plan or description defers it. Fail on "tested locally", "works now", "verified on my machine", or "tests pass" with no observable result. Fail too if only a control or the unchanged path was exercised, or if one of the plan's named failure scenarios is silently missing. | required |
| repo-checks-run | The test evidence, read against the suite or check commands that the plan's test plan or the repo facts name (live: the PR template's Testing list `make test-unit`, `make test-integration`, `make lint`, `make typecheck`). | Every suite or check command the plan or repo names appears in the evidence with its outcome: passed with a count, or a failure or "no tests ran" reported honestly with its reason. Pass trivially when neither the plan nor the repo names any command. Fail if a named command is never mentioned, or only a vague "tests pass" appears where a named command was asked for. | required |
| diff-free-of-debris | Every `+` and `-` line in the diff, plus the commit messages (live: `git diff main...HEAD`, `git log --oneline main..HEAD`). | The diff contains only the change and its tests or docs. Fail on any of these: a debug print or log left in (`eprintln!("DEBUG`, `print(`, `console.log`, `dbg!`); a commented-out code block or earlier attempt; a dead function or variable (unused, or kept alive with `allow(dead_code)` or an `_unused` name); a stray TODO or FIXME added; formatting churn (re-indented, re-wrapped, or re-printed lines with no change in meaning); an import reshuffle the change does not need. An explanatory comment about the fix is not debris. | required |
| template-asks-met | The repo's stated PR asks (eval: the repo-facts block's "pull requests" line; live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and `scope.md` house rules), read against the description and the diff. | Every concrete ask that applies to this PR is met. Concrete asks are a named template section, a "closes/fixes #N" line or issue reference, a required checklist, a CHANGELOG or whatsnew entry, an added test, or before/after for a UI change. A section is met when it has real content, an ask is met when the description shows it or states why it does not apply ("no UserConfig changes"), and a file ask is met when the file is in the diff. Pass when the repo states no asks. Fail when an applicable ask is visibly ignored: a required checklist absent, a required "closes #" line missing, a required changelog or whatsnew entry missing from the diff, a template section empty or left as placeholder. Fail too in live mode when a Testing box is ticked for a check the evidence does not show. Do not grade adjectives ("release-notes quality"). | required |
| ai-disclosure | The repo's stated AI policy (eval: the repo-facts "contribution policy" line; live: CONTRIBUTING, any AI_POLICY file, and the course's standing rule that every PR discloses AI use in Notes for Reviewers), read against the description. | If the policy requires disclosing AI use in PRs ("all AI usage in any form must be disclosed"), the description itself states that AI was used, the tool or extent, and that the human understands the work. The course's standing rule always requires this in live mode. If the stated policy asks for no PR disclosure, this passes with nothing to do. A policy that only asks for own-words comments or human understanding is not a disclosure requirement. Fail when disclosure is required and the description has none. | required |
| commits-describe-change | The commit list. | Each commit message names what it changes ("parser: reserve stream space ..."), not "wip", "fix", or "misc cleanups". | preferred |

## Verdict rule

Accept when all seven required checks pass. Reject when any required
check is `fail` or `unclear`. `unclear` counts as fail, because a PR
whose fidelity, proof, or compliance cannot be verified from the
package is not ready to submit. `commits-describe-change` is preferred:
report its grade, and never use it in the decision.

Two clarifications:

- **Short is not failing.** A one-hunk diff with a two-sentence
  description, the repro flipping before/after, and the repo's asks
  met passes every required check.
- **Disclosed shortfalls pass.** A PR that does less than everything
  can be accepted when the plan deferred the rest, or a deviation note
  records it, and the description restates it. "Less than the plan" is
  drift only when nobody said so.
