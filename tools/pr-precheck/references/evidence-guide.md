# Evidence guide: where evidence lives in a PR package

For each evidence family: where it lives in an eval bundle, where it
lives in live mode (your own PR on Path Review), and what good looks
like. The four families are the harness's failure categories. A package
that fails none of them is a clear accept.

## Plan fidelity (harness category: silent-drift)

Checks: `diff-matches-plan`, `description-matches-diff`.

**Where it lives (eval).**

- **What was promised:** the Plan context block. The "Plan:" paragraph
  gives the scope ("Scope, one bounded change: ..."), the "Not in
  scope:" sentence, the "Files:" list, and any "Deliberately deferred"
  or "Known limit" note.
- **What was delivered:** the Diff under Candidate PR. Each
  `+++ b/<path>` line names a changed file, and each `@@` hunk is one
  change.
- **What is claimed:** the Title and Description under Candidate PR.
  Look for any "Deviation note" in the description.

**Where it lives (live).**

- `plan.md`: its Scope (in scope and not in scope), Files I'll touch,
  Approach, Risks, and the `## Deviations` section.
- `git diff main...HEAD` and `git diff main...HEAD --stat` for what
  the branch changes.
- `pr_draft.md` for the title (first line) and the Summary and Changes
  sections.

**What good looks like.** Every changed file and hunk maps to a planned
change, its test, a repo-required file, or a deviation note. Every
planned change is in the diff, or is deferred out loud. Every claim in
the description points at a hunk you can find.

**Silent drift looks like:**

- More than the plan: a rewrite of a neighboring function, a rename
  pass, a new config option or flag, a dependency bump, or a hunk in a
  file the plan never names, under a description saying "exactly as
  planned".
- Less than the plan: a planned docs update or second fix missing, with
  no deferral note, while the description claims it was done.

An honest deviation re-ties the mismatch: a note in the plan (live:
`## Deviations`) and a restatement in the description.

## Test evidence (harness category: not-tested)

Checks: `repro-rerun-decisive`, `repo-checks-run`.

**Where it lives (eval).**

- **The bar:** the Plan context's "Repro evidence" line (the failing
  input and what it produced) and its "Test plan:" sentence (each
  scenario, its expected-after, and any suite command like `cargo test`
  or `yarn test`).
- **The proof:** the Test evidence section under Candidate PR, plus any
  before/after in the Description.
- **Checks the repo asks for:** the repo facts' "pull requests" line
  ("pass the test suite (yarn test)").

**Where it lives (live).**

- **The bar:** `plan.md`'s `## Test plan` (Runs 1-4 with their Before
  and Expected after), and the issue's Unit 2 repro comment.
- **The proof:** `test_evidence.md` (the repro re-run on `main` and on
  the branch, and the four template checks) and the Testing section of
  `pr_draft.md`.
- **Checks the repo asks for:** the PR template's Testing list,
  `make test-unit`, `make test-integration`, `make lint`, and
  `make typecheck`. `make test-integration` reporting "no tests ran"
  is the expected, honest result.

**What good looks like.**

- The repro's own failing input is re-run on the branch, with an after
  result that differs from the repro's Actual result and matches the
  plan's expected-after (a value, output, exit code, or a named test
  passing).
- Every failing scenario the test plan names is shown or explicitly
  deferred.
- Each named check command appears with its outcome, failures included,
  each with its reason.

Not decisive: "tested locally", "works now", "verified for a full day",
"`cargo test` passes" standing in for the repro, or a re-run of only
the control or the unchanged path.

## Diff quality (harness category: unreviewable)

Checks: `diff-free-of-debris`, plus the preferred
`commits-describe-change`.

**Where it lives (eval).** The Diff (every `+` and `-` line) and the
Commits list under Candidate PR.

**Where it lives (live).** `git diff main...HEAD` (committed changes
only) and `git log --oneline main..HEAD`.

**What good looks like.** Every changed line serves the fix, its test,
or its docs, and the fix is easy to find. Commit messages name what
each commit changes.

**Debris tells:**

- debug output left in (`eprintln!("DEBUG ...")`, `print(`,
  `console.log`, `dbg!`);
- commented-out code or a commented-out first attempt;
- a dead function or variable (`_unused_...`, `#[allow(dead_code)]`);
- an added TODO or FIXME;
- re-indented or re-wrapped untouched lines, or identical lines
  removed and re-added;
- an import reshuffle the fix does not need;
- commits named "wip", "fix", or "misc cleanups while debugging".

A comment that explains why the fix is there is not debris.

## Standards and comms (harness category: standards-wall)

Checks: `template-asks-met`, `ai-disclosure`.

**Where it lives (eval).** The Repo facts block:

- the "pull requests" line, which carries template sections,
  checklists, "closes #xxxx", and CHANGELOG or whatsnew asks;
- the "contribution policy" line, which carries any AI policy
  ("All AI usage in any form must be disclosed").

The PR meets them in the Description (sections, issue line, checklist,
disclosure) and the Diff (a CHANGELOG.md or whatsnew file in the
changed files).

**Where it lives (live).**

- `.github/PULL_REQUEST_TEMPLATE.md`: Summary, Issue (`Closes #`),
  Changes, Testing checklist, Screenshots / Demo, and Notes for
  Reviewers.
- `docs/CONTRIBUTING.md`: the branch naming
  `<type>/<issue-number>-<short-description>`, the conventional
  commits, the rule to remove a seeded bug's `@pytest.mark.xfail`
  marker and matching `pyproject.toml` suppression, and the ban on
  bulk lint fixes.
- `scope.md` house rules.
- The course's standing rule: an AI-use disclosure in Notes for
  Reviewers.

The PR meets them in `pr_draft.md` and the diff.

**What good looks like.**

- Every applicable ask is visibly met, or answered with why it does not
  apply.
- Every template section has real content, not the placeholder comment.
- The issue is linked with the line the repo asks for.
- Required files are in the diff.
- Only true boxes are ticked.
- Where disclosure is required, the description says in the author's
  own words that AI was used, how, and that they understand the
  change.

A visibly ignored ask looks like a missing checklist, a missing
`closes #` line, a missing whatsnew entry, or no disclosure under a
must-disclose policy.

Whether the description's claims match the diff is plan fidelity, not
this family.
