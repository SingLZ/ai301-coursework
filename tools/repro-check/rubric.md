# Rubric: is this reproduction package ready to post?

Seven checks. Five read the proof (environment, steps, artifact,
target, honesty); two read the words that go out with it (the claim
comment, the repo's AI policy). All seven are required: a package can
be technically perfect and still get the door shut on it for what the
comment promises or fails to disclose, and that is a package that was
not ready to post.

Assumption the checks run on: every package graded here is
AI-assisted work, because I used an assistant to prepare it. Where a
repo's stated policy asks for disclosure, "no AI was involved" is
never the reading.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment record, read against the version/platform the issue targets (issue body, thread highlights) and the repo-facts template asks. | The report names the version it ran (release number, commit, or build id) AND the platform (OS/arch, plus the shell, browser, install channel, or build profile when the issue's failure depends on one), AND that version/platform is what the issue targets, or the report says outright how it differs ("filed against 3.8.4, still reproduces on 3.9.6"; "report is macOS + fish, I ran Linux + zsh"). Fail if there is no environment record at all, or the run sits on a different version/platform and the report never says so. | required |
| steps-rerunnable | The steps section read as a stranger holding only this package would follow it, end to end, from their own machine. | The steps are an ordered sequence from a stated starting point (empty directory, fresh install, the issue's own file or playground link) in which every action is concrete enough to repeat — exact commands, exact keystrokes or UI actions, config and input content given inline — and every asset needed is in the package. Fail if any step requires an action the reader has to infer ("ran the usual setup", "our pre-commit hook") or an asset they cannot obtain (private repo, unshared config). | required |
| behavior-shown | The artifacts pasted in the repro report: command output, error text, stack trace, exit status, log excerpt, produced file, or the content of a screenshot. | At least one verbatim artifact of the run's outcome is in the report — the thing the machine printed, not the writer's summary of it. Fail if actual behavior exists only as prose ("it crashed", "same as the issue", "the screenshot shows the bug"), or if the only artifacts are setup evidence (version banners, session lists, "the tool started") that show the run happened but not how it ended. | required |
| behavior-matches-issue | The pasted artifact read line by line against the failure the issue describes: its error type, message, exit code, or symptom, and the trigger that produced it. | The artifact shows the same failure the issue reports, produced by the issue's trigger — OR the report states plainly that what it got differs and how, which includes an honest cannot-reproduce that names the attempt and what differed. Fail if the artifact shows an adjacent failure (a graceful validation error where the issue reports a panic, a compile error where the issue reports a runtime one, garbled output where the issue reports a crash), or the run altered the trigger (different syntax, different expression, different version) so the artifact is evidence about something else, while the report presents it as the issue's behavior. | required |
| verdict-honest | The report's stated result and conclusion, plus the certainty in the claim comment, read against what the artifacts actually establish. | The stated verdict — reproduced, partially reproduced, or cannot reproduce — is what a reader would conclude from the artifacts alone, with scope limits stated where they exist ("scenario 2 only", "did not test scenario 1"). Fail if the prose outruns the evidence: "confirmed", "guaranteed reproducible", or "verified" over artifacts that do not show the failure; a root cause asserted with no test shown; or a generalization to another build, release, or platform that was never run. | required |
| claim-comment-specific | The candidate claim comment, read against the issue it is answering and the repo-facts block's template and policy asks. | The claim names what this person did or saw in this issue — the version they ran, the behavior or the attempt, in their own terms — and states a concrete next step, and it promises no delivery date and no guaranteed outcome. Fail if the comment is interchangeable text that would fit any issue ("+1", "this bug", "kindly assign it to me"), or if it promises a timeline or a guarantee ("I will fix it within 2 days"). | required |
| ai-disclosure | The repo-facts contribution-policy line (CONTRIBUTING.md, AI_POLICY.md, AI usage sections) read against both outgoing comments. | Either the repo's stated policy asks for no AI disclosure on issue comments — then this passes with nothing to do — or the policy does ask, and the comments disclose the assistance and the human's ownership of the work ("I used an AI assistant to help organize this report; I ran and verified every step myself"). Fail only when the stated policy requires disclosure and neither the claim comment nor the report discloses. A policy that merely demands the contributor understand their work is not a disclosure requirement; a policy saying all AI usage in any form must be disclosed is. | required |

## Verdict rule

Accept when all seven required checks pass. Reject when any of them
fails. There are no preferred checks in this rubric, and `unclear`
counts as a fail: proof I cannot verify from the package is proof that
is not ready to post.

Two clarifications the checks depend on. An honest cannot-reproduce is
an accept, not a reject: `behavior-matches-issue` is satisfied by a
report that names the attempt, shows its artifacts, and states what
differed from the issue's conditions, and `verdict-honest` is satisfied
by a negative verdict the artifacts support. And a failure in
`claim-comment-specific` or `ai-disclosure` rejects on its own, even
over flawless proof: an over-promising claim or an undisclosed AI-
assisted comment in a repo that requires disclosure is a package that
costs its author the thread, whatever the artifacts show.
