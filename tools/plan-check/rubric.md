# Rubric: is this plan ready to post and build from?

Six required checks and one preferred. Four read the plan itself against
the evidence (cause, scope, executability, test); two read the words
that go out with it against the room they go into (the thread's
maintainer direction, the repo's AI policy). The preferred check reads
honesty about unknowns and never changes the verdict.

Assumption the checks run on: every package graded here is AI-assisted
work. Where a repo's stated policy asks for AI disclosure, "no AI was
involved" is never the reading.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause (its Diagnosis or Cause line, and any cause asserted in the plan comment), read against every step, control run, timing, and artifact in the Repro evidence block. Thread claims are context, not evidence. | The stated cause explains every observation the repro evidence records, including each control run: the bug appears exactly where the blamed code runs, and disappears where it does not. Fail if any repro step or control contradicts the cause, such as the failure happening with the blamed component out of the loop, the blamed component working correctly in a control, or the artifact showing the failure already present before the blamed code runs. Fail too if the cause is adopted from the thread or asserted with no link to the repro at all. A cause that agrees with a confident thread comment but not with the repro evidence fails. | required |
| scope-bounded | The plan's scope statement (in-scope and not-in-scope lines) and its list of changes or approach steps, read against the failure the issue and repro describe. | Every planned change is needed to fix the reproduced failure, to test it, or to document the changed behavior, and the plan says what it will not touch. Deferring a related variant or a deeper rework, with a reason, is bounded and passes. Fail if the plan bundles work the issue never asked for alongside the fix: refactors or rewrites of surrounding code, dependency upgrades or migrations, new options or settings, UI rework, new frameworks, CI or test-harness changes beyond the regression test. It fails even when the core fix inside is correct. | required |
| executable | The plan's files, functions, or sites named, and its approach or change steps. | A stranger holding only this package could start the change without asking the author anything: the plan names where the change goes (a file, function, branch, or call site) and commits to one approach. Fail if the location is "somewhere" or unnamed, the approach is a choice left open ("upstream or vendored, whichever is easier", "gocui? tcell? not sure"), or the plan's first step is to investigate or profile and decide later. | required |
| test-decisive | The plan's test plan, read against the Repro evidence block's steps and its Expected/Actual lines. | The test plan names an observable outcome that would differ between the broken and fixed code, for the failure the repro shows. Re-running the repro steps with a stated expected result passes, as does a regression test that asserts the reproduced case. Fail if the only test is a generic regression run ("run the full suite and make sure nothing regresses"), a feeling ("should feel fast", "nothing else should feel broken"), or a check that would pass on the unfixed code. | required |
| thread-direction | The plan comment and the plan, read against the Thread highlights entries from OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR-role maintainers (live: the issue thread). | Either no maintainer has given direction in the thread, or the plan comment engages it: it follows the direction, or names it and says why it diverges. Direction means a maintainer isolating a culprit or file, proposing or rejecting an approach, posting a patch or test build, or asking for a specific next step. Fail if such direction exists and the comment neither follows it nor mentions it, for example a workaround or docs plan where the owner has already isolated the code culprit and asked for testing. Comments from non-maintainers (NONE role) are not direction. | required |
| ai-disclosure | The Repo facts block's contribution-policy line (CONTRIBUTING.md, AI_POLICY.md, AI sections), read against the plan comment. | Either the stated policy asks for no AI disclosure in comments, and this passes with nothing to do, or it does ask, and the plan comment itself discloses the AI assistance (the tool or the fact of assistance, and its extent) and the human's ownership of the work. Fail only when the policy requires disclosure ("all AI usage in any form must be disclosed") and the comment has none. A policy that only requires the contributor to understand the work, asks that comments be in the contributor's own words, or limits disclosure to pull requests is not a comment disclosure requirement. | required |
| unknowns-stated | The plan's risks/unknowns section, and the certainty words in the plan and comment, read against what the repro evidence establishes. | The plan names what it has not verified (an untested platform, an unmeasured cost, an open design question) rather than presenting it as settled. | preferred |

## Verdict rule

Accept when all six required checks pass. Reject when any required
check fails. `unclear` counts as fail: a plan whose cause, scope, test,
or comms cannot be verified from the package is not ready to build
from. `unknowns-stated` is preferred and never changes the verdict;
report its grade, but leave it out of the decision.

Two clarifications. A short plan is not a failing plan: a few lines
that state a grounded cause, name the one site, bound the change, and
re-run the repro with an expected result pass every required check.
And a long, polished, confident plan gets no credit for polish: if its
cause contradicts a control run, or it wraps the fix in a redesign, it
is held.
