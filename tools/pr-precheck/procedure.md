# Procedure: how this tool grades a PR package

These are steps for grading a PR, not for writing one. Follow them in
order. Where a step names a part of the package, it means the section
with that heading in an eval bundle. In live mode it means the file or
command `references/evidence-guide.md` and SKILL.md name for that part.

## Read order

1. Read `rubric.md` and `references/evidence-guide.md`. Write down the
   eight check names in rubric order, which seven are required, and the
   verdict rule.
2. Live mode only: read `scope.md` first, and apply SKILL.md's scope
   seam (stop on a placeholder `Repo:` line, refuse an out-of-scope
   repo). Then gather the live inputs SKILL.md lists: `plan.md`,
   `git diff main...HEAD`, `git log --oneline main..HEAD`,
   `git branch --show-current`, `pr_draft.md`, `test_evidence.md`, the
   issue thread, `.github/PULL_REQUEST_TEMPLATE.md`, and
   `docs/CONTRIBUTING.md`.
3. Read the repo facts (eval: the Repo facts block; live: the template
   and CONTRIBUTING). Copy, word for word, every concrete PR ask: each
   template section, any "closes #" line, any checklist, any changelog
   or whatsnew file, any test ask, and any named check command. Then
   copy the AI policy sentence, and note whether it requires PR
   disclosure.
4. Read the Issue and Thread highlights. Note the issue number and, in
   one sentence, the failure being fixed.
5. Read the plan context (eval: Plan context; live: `plan.md`) before
   any of the candidate PR. Write down:
   - the planned changes, as a numbered list;
   - the files the plan names;
   - the not-in-scope line;
   - every deferral or deviation note;
   - each test-plan item: the failing scenario, its expected-after, and
     any suite command;
   - the repro's failing input and its Actual result.

   This list is fixed before you see the PR, so the description's story
   cannot shape what you think the plan promised.
6. Read the Diff next, before the description. List every changed file
   and every hunk, with a few words on what each hunk does.
7. Read the Commits, then the Test evidence.
8. Read the Title and Description last. Read them as claims to be
   checked against what you already listed, not as a summary to trust.

## Evidence gathering

1. **Diff against plan** (diff-matches-plan). For each hunk from Read
   order step 6, write which planned change it implements, tests, or
   documents, or which deviation note covers it. Otherwise write
   "unplanned". Then, for each planned change from step 5, write the
   hunk that delivers it, or the deferral note that covers it, or
   "missing".
2. **Description against diff** (description-matches-diff). List each
   factual claim in the title and description: what changed, files,
   docs, tests, changelog, and fidelity phrases like "exactly as
   planned", "no other changes", or "only". Next to each, write the
   hunk that shows it, or "not in diff", or "contradicted by <hunk>".
3. **Evidence against test plan** (repro-rerun-decisive). For each
   test-plan item from step 5, copy the evidence line that shows its
   after result, or write "not shown". Note whether that evidence runs
   the failing input, or only a control or a different path. Compare
   the after result to the repro's Actual result, and write "differs"
   or "same".
4. **Checks against asks** (repo-checks-run). For each suite or check
   command the plan or repo names, copy the evidence line with its
   outcome, or write "not shown".
5. **Debris scan** (diff-free-of-debris). Go through every `+` and `-`
   line, and copy each debug print, commented-out code line, dead
   function or variable, added TODO or FIXME, re-indent or re-wrap with
   no change in meaning, and unneeded import reshuffle. Write "none" if
   there are none.
6. **Asks against the PR** (template-asks-met). For each ask from Read
   order step 3, write "met" with the line or file that shows it, "N/A"
   with the reason the ask cannot apply, or "ignored". Live mode: also
   compare each ticked Testing box in `pr_draft.md` to
   `test_evidence.md`, and confirm the branch name matches
   `<type>/<issue-number>-<short-description>`.
7. **Disclosure** (ai-disclosure). If the policy (live: always)
   requires PR disclosure, copy the description sentence that discloses
   AI use, or write "none".
8. **Commits** (commits-describe-change). Copy any commit message that
   does not name its change, such as "wip", "fix", or "cleanup".

## Check execution

1. Grade the checks in rubric order: diff-matches-plan,
   description-matches-diff, repro-rerun-decisive, repo-checks-run,
   diff-free-of-debris, template-asks-met, ai-disclosure,
   commits-describe-change.
2. For each check, compare only the notes gathered for it against the
   pass condition in `rubric.md`. Grade `pass`, `fail`, or `unclear`,
   and write the one fact or quote that decided it.
3. diff-matches-plan fails if any hunk is "unplanned" and changes code
   or behavior, or if any planned change is "missing". A formatting or
   debug-only hunk is graded by diff-free-of-debris, not here.
4. description-matches-diff fails on any "not in diff" or
   "contradicted" claim.
5. repro-rerun-decisive fails on any "not shown" test-plan scenario
   that has no deferral note. It also fails when the shown result is
   only a control or another path, or when it is "same".
6. repo-checks-run and template-asks-met fail on any "not shown" or
   "ignored". "N/A" with a reason that follows from the ask's own
   condition is not a fail.
7. Grade `unclear` only when the package truly lacks the part a check
   reads (no test evidence section at all, no plan at all), never
   because you skipped a step. The verdict rule counts `unclear` as a
   fail.
8. Keep each check separate. Decisive evidence does not rescue an
   unplanned hunk, and debris does not fail fidelity. Do not fail a
   check because the description is short, and do not pass one because
   it is long and confident.

## Verdict assembly

1. Take the seven required checks. If all seven are `pass`, the verdict
   is `accept`.
2. If any required check is `fail` or `unclear`, the verdict is
   `reject`.
3. Leave `commits-describe-change` out of the verdict, and report its
   grade only.
4. The deciding check is the first failing or unclear required check in
   rubric order. In the summary, name it and quote its evidence. Then
   list any other failing checks, each with one line.
5. Live mode only: once the verdict is decided, hold the title and
   description against `voice-guide.md`, and list any broken rule in
   the summary. This never changes the verdict.
6. Emit the JSON block exactly as SKILL.md specifies, with all eight
   checks in rubric order, as the last thing in the output.
