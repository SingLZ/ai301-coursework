# Evidence guide: where evidence lives in a plan package

The map the rubric's checks read from. Each family says where to look
(eval package first, then live) and what good looks like there.

## Diagnosis and grounding

Where it lives. In a package: the plan's `Diagnosis` or `Cause:` line
(or, in an unheaded plan, the first sentence saying why the bug
happens), any cause the `## Candidate plan comment` asserts, and the
`## Repro evidence` block: its numbered steps, timings, control runs,
`Artifact:` blocks, and the closing `Expected:` / `Actual:` lines. The
`## Thread highlights` may contain cause claims too, and they are
context, not evidence. Live: the draft plan's diagnosis, and the
student's posted repro comment on the issue (or the repro quoted in the
drafts).

What good looks like. The cause predicts every row of the repro: it is
present where the failure happens and absent where the controls pass.
Look hardest at the controls (calib-03's step 3 runs with no pager at
all and is still slow, so a pager-binding cause cannot be right). A
cause copied from a confident thread comment, or from the issue title,
with no line back to a repro step, is not grounded. A cause that
matches the repro's traceback location and its control run is.

## Scope

Where it lives. In a package: the plan's `Scope` section ("In scope:" /
"Not in scope:" lines), its `Changes` or `Approach` list, and stray
"also" or "while I'm in there" sentences anywhere in the plan or
comment. Live: the same in the draft plan's scope and files sections.

What good looks like. One bounded change: each item fixes or tests the
reproduced failure, and a not-in-scope line names the nearby work being
left alone. Deferring a deeper rework or an untestable platform variant
with a reason is a good sign. A drive-by plan reads differently: the
fix sits inside a dependency migration, a settings panel, a module
restructure, a state-machine rewrite, or a CI matrix. It fails even
when the one real fix inside it is right.

## Executability

Where it lives. In a package: file paths, function names, and call
sites in the plan's `Diagnosis`, `Scope`, `Changes`, or `Approach`; and
the verbs in those steps. Live: the draft plan's files-to-touch and
approach sections.

What good looks like. A stranger could open the named file and start:
"the push completion callback in
`pkg/gui/controllers/sync_controller.go`" or "saturating clamp at the
two subtraction sites". Vague looks like no files, open choices ("gocui?
tcell?"), or "recover() somewhere", or a first step that is to
investigate or profile and decide later.

## Test plan

Where it lives. In a package: the plan's `Test plan` or `Test:` line,
read against the repro's steps and its `Actual:` line (the unfixed
result). Live: the draft plan's test plan, against the steps in the
student's repro comment.

What good looks like. It names a result that is different after the
fix. Examples: "at step 3 the color must flip without leaving the
view", "repro re-run exits 0", "both fuzz cases pass as regression
tests and the control is unchanged". A vague plan names nothing the fix
changes: "run `cargo test --workspace` and make sure nothing
regresses", "should feel fast".

## Honesty

Where it lives. In a package: a `Risk`, `Risks`, or `Unknowns` section;
hedges in the approach ("I have not measured"); certainty words in the
comment ("I traced this", "guaranteed"). Live: the same, plus the
`## Deviations` section of `plan.md` after the build.

What good looks like. It names what has not been checked: an unmeasured
cost, an untested OS, an open question for review. False confidence is
"I traced this" above a cause the repro contradicts, or a guarantee no
test supports. A deviation found mid-build is honest when `plan.md`'s
Deviations section records what changed and why.

## Comms

Where it lives. In a package: the `## Candidate plan comment`, read
against two places. First, `## Thread highlights`: entries marked
OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR, where a maintainer
isolates a culprit, proposes or rejects an approach, posts a patch or
test build, or asks for testing. Second, the `## Repo facts` block's
`contribution policy:` line, for AI disclosure rules. Live: the draft
comment, the live issue thread's maintainer comments, and the repo's
CONTRIBUTING.md and AI policy files.

What good looks like. The comment shows it read the thread. It follows
the maintainer's direction ("my plan follows the direction proposed
here: recompute only when capacity changed") or names it and says why
it diverges. Boilerplate ignores it. pkg-04's docs-workaround comment,
for example, never mentions that the owner already isolated the culprit
in `src/tui/light_windows.go` and posted a test build. For AI policy,
sort into two buckets. No disclosure ask: no policy, a policy that only
asks contributors to understand their work or write comments in their
own words, or disclosure limited to PRs. That passes with nothing to
do. Disclosure required (ghostty's "all AI usage in any form must be
disclosed"): the comment itself must say AI was used, to what extent,
and that the human owns the work.
