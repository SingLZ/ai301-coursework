# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/106

**Branch**

fix/68-empty-index-guard

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Partial run (`--only calib-03,calib-04,pkg-01,pkg-13,pkg-16,pkg-20 --include-calibration`):
   3/4 scored items agreed (`agreement: 3/4 scored items`). Both calibration
   packages, calib-03 and calib-04, were rejected, which matches their gold labels.
   pkg-16 (clear-accept) came back reject, with the note
   `failed: description-matches-diff`.
2. Partial run (`--only pkg-16`), with no file changes: 1/1. pkg-16 was
   accepted, with all eight checks passing, so the first reject was run-to-run
   noise. I think the trigger was the description's context lines (the "Before:"
   transcript and "discussed with @nirs on the thread"). Before the full run, I
   tightened description-matches-diff so that context like this is never graded
   as a claim about the diff.
3. Full run (`--save-run eval-run.txt`): **19/20**. The file's agreement line is
   `agreement: 19/20 scored items  (bar: 18/20: PASS)`, and its categories line is
   `categories: clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.
   This is the run in `eval-run.txt`.

**Package analysis**

**pkg-05** (nushell/nushell#18848, clear-accept). My rubric: **reject**. Gold
label: **accept**. The check it failed was repro-rerun-decisive.

The plan's test plan names two scenarios: "re-run the issue's script, expect
both `atuin` rows; re-run with two same-name same-key bindings, expect one row
plus a warning". The PR's evidence shows the first scenario decisively, with a
before table holding one `atuin` row and an after table holding both rows. The
second scenario only gets one sentence of prose: "Same-key redefine prints the
one-time warning." There is no output and no named test.

My check says "Every distinct failing scenario the test plan names must be
shown, unless the plan or description defers it". So the grader failed it, with
this evidence line: "the plan's second scenario (same-name same-key: one row
plus a warning) is only asserted in prose with no output and the named test is
not cited as passing."

I think gold accepts it because the second scenario is a check on new behavior
(the warning), not the bug being reproduced. The actual bug visibly flipped,
and `cargo test -p nu-protocol` passes (312 tests). My rule treats every
scenario the plan lists the same way, so it can't tell the two apart.

**Check rationale**

From `tools/pr-precheck/rubric.md`, exactly as it reads now:

> | repro-rerun-decisive | The test evidence section and the description's before/after (live: `test_evidence.md` and `pr_draft.md`), read against the plan's Test plan and the repro evidence's failing input and Actual result. | The evidence re-runs the failing case the plan's test plan names (the repro's own input, not a control or a different path) on the branch. It shows an observable result that differs from the repro's Actual result, and it matches the plan's expected-after: output, exit code, a printed value, or a named test asserting that exact case passing. A before line may come from the repro evidence. Every distinct failing scenario the test plan names must be shown, unless the plan or description defers it. Fail on "tested locally", "works now", "verified on my machine", or "tests pass" with no observable result. Fail too if only a control or the unchanged path was exercised, or if one of the plan's named failure scenarios is silently missing. | required |

Why it reads this way: the not-tested packages fail in different ways, so each
clause targets one of them.

- **"the repro's own input, not a control or a different path"** catches pkg-07
  (the evidence runs only the single-file control, which never breaks) and
  pkg-14 (the evidence exercises a GET with header casing, not the issue's POST).
- **The banned phrases** ("tested locally", "works now", "verified on my
  machine") catch pkg-04 ("tested locally, colors work now") and pkg-10
  ("verified working on my machine for a full day").
- **"Every distinct failing scenario … must be shown"** catches calib-04, where
  the plan names two failure modes and the evidence re-runs only one.
- **"unless the plan or description defers it"** keeps the honest shortfalls
  passing: pkg-13 defers `%VAR%` expansion and pkg-16 defers the start path,
  and both are gold accept.

I chose an outcome rule over "the description has a Testing section", because a
section can exist and still prove nothing.

**Trade-offs**

The "every named scenario" clause costs me pkg-05. My rubric rejected it, while
gold accepts it, because its second test scenario is only described in prose.

I considered loosening the clause to "the primary repro only", but decided
against it. A looser version would probably start accepting calib-04-style PRs,
which quietly skip a second failure mode the plan promised to test. Not-tested
is a category of only 4 packages, so I'd rather wrongly hold a PR that left out
a secondary check than let through one that skipped a real failure. I accept
that this check will miss cases like pkg-05, where a plan lists an extra
behavior check alongside the bug. I made no change for it, so no canary re-run
was needed. The only edit between runs was to description-matches-diff, and
the full run confirmed nothing else flipped: every category held at least one
match (not-tested 4/4, standards-wall 2/2).

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
