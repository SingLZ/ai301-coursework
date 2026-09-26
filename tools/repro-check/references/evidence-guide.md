# Evidence guide: where proof lives in a reproduction package

The map the rubric's checks read from. Each family says where to look
first (eval bundle, then live), and what good looks like when you get
there.

## Environment

Where it lives. In a bundle: the repro report's first lines, usually an
`Environment:` line or a version table; the issue's own version and
platform statement near the top of the `## Issue` section, and any
thread highlight where a maintainer narrows the failure to a build,
channel, or platform; the `bug reports:` line in the repo-facts block,
which names the fields that repo's template asks for. Live: the draft
report, the issue body's template fields, and the repo's issue template
in `.github/ISSUE_TEMPLATE/`.

What good looks like. A version the reader could install (release
number, commit, or build id) plus the platform, and enough of the
dimension the failure turns on that a stranger could match it — the
shell for a prompt bug, the browser and language order for a frontend
bug, the driver for a VM bug, the build profile when debug and release
differ. When the run is not on the version the issue names, good looks
like the report saying so in a sentence and saying what it means
("filed against 3.8.4; both shapes still reproduce on 3.9.6"). A bare
"latest version, my machine" is not a record. No environment line at
all is the common failure, and it is fatal even when the artifact below
it looks perfect.

## Steps

Where it lives. In a bundle: the `Steps:` block of the repro report,
plus any setup sentence before it and any control run after it; read it
against the issue's own steps or minimal reproduction, which is the
trigger the steps are supposed to hit. Live: the draft's steps section
and the issue's reproduction link or snippet.

What good looks like. A numbered or otherwise ordered sequence that
starts from a state the reader can create — an empty directory, a fresh
install, the issue's file pasted inline, the official playground — and
whose every action is something the reader can perform without
inventing anything: a command with its flags, a keystroke and what was
focused when it was pressed, a config file whose contents are shown. A
GUI step is fine when it is exact ("focus new.txt in Files, press `s`,
name it test, Enter"). Two shapes are not followable, however polished
the surrounding report: a step whose input is private or unshared ("our
monorepo, which I cannot share", "our internal .golangci.yml"), and a
step that hides work in a phrase ("ran our pre-commit hook the same way
the reporter does"). A control run — the same steps with the one
triggering detail changed — is not required, but it is the strongest
signal that the steps isolate the trigger rather than surround it.

## Behavior shown

Where it lives. In a bundle: the fenced blocks in the repro report, and
the `Actual:` line that interprets them. Live: the draft's pasted
output, log excerpt, or attached screenshot.

What good looks like. The machine's own words, verbatim: the error and
its type, the stack or traceback lines, the exit status, the repeating
log lines, the produced file or rendered output. The artifact has to be
of the outcome, not of the setup: a version banner, a session list, or
"the tool launched with all three tabs visible" proves the run started,
not how it ended, and a report whose only fenced block is setup has
shown nothing about the bug. A screenshot counts when its content is
described concretely enough to check against the issue; "the screenshot
shows the bug" is prose. Prose alone — "it crashed", "same as the
issue", "reproduces every time" — is the failure this family exists to
catch.

For whether the artifact shows the *right* behavior, read it against
the issue's failure: the error type and message, the exit code, the
specific line of stack trace, the symptom the reporter named. An
adjacent failure is the trap, and it usually reads as a success: a
graceful argument-validation error (exit 1) where the issue reports a
capacity-overflow panic (exit 101); a compile error where the issue
reports a runtime path error; garbled escape output with the terminal
still alive where the issue reports a crash; the old version's
`ValueError` where the issue is confirmed on main. Check the trigger
too — if the run changed the expression, the syntax, or the version,
the artifact is evidence about the changed thing, and the report has to
say so.

## Honesty

Where it lives. In a bundle: the report's opening `Result:` or summary
line and its closing conclusion, the `Expected:`/`Actual:` pair, and the
certainty words in the claim comment; read all of them against the
fenced blocks above them. Live: the same places in the draft, plus
anything the draft asserts about code it did not run.

What good looks like. The verdict and the artifact say the same thing.
A report that reproduced says so and shows it. A report that partially
reproduced names the part ("scenario 2 only; I did not test scenario
1"). A cannot-reproduce is a full pass on this family when it shows the
real attempt, its artifacts, and what differed from the issue's
conditions, with a hypothesis marked as one — that report tells a
maintainer where the boundary is, which is real information. The
failures are all overclaims: "confirmed" or "guaranteed reproducible"
above an artifact that does not show the failure; a root cause named as
fact ("it is the debounce race, I verified it") with no test shown;
"expected" and "actual" written backwards from what the block prints;
and the quiet generalization — claiming a result on a release, build,
or platform the run never touched.

## Comms

Where it lives. In a bundle: the `## Candidate claim comment` section,
plus the repo-facts block's `bug reports:` line (what the template asks
for) and `contribution policy:` line (contribution guide, AI policy,
disclosure rules, vouch flows). Live: the draft comment, the repo's
CONTRIBUTING.md and any AI_POLICY.md or AI-usage section, and the issue
thread's existing comments.

What good looks like. A claim comment that could only have been written
about this issue by this person: the version they ran, the behavior or
the failed attempt they saw, and one concrete next step they intend to
take, in their own voice. What fails is the interchangeable comment
("+1", "this bug", "great project, kindly assign it to me"), the
comment with no stated intent behind the claim, and any promise the
writer cannot keep — a delivery date, a guaranteed fix, a reserved
issue.

The AI policy is a separate gate, and it is read from the repo, not
from taste. Sort the stated policy into one of two buckets. No
disclosure ask: no policy at all, or a policy that only requires the
contributor to understand and take responsibility for what they submit
(conda's "you are responsible for all contributions", prettier's "only
submit code you fully understand"), or one that scopes its disclosure
ask to pull requests and says nothing about issue comments (fd). Those
pass with nothing to do — though a policy asking that comments be in
the contributor's own words still fails a comment that reads as
generated boilerplate. Disclosure required: the policy says AI usage
must be disclosed, with the tool and the extent of its help (ghostty's
"all AI usage in any form must be disclosed"; p5.js's AI usage policy,
which accepts assistive use and expects the contributor to own the
work). There, the comment has to say it, in the comment itself: "I used
an AI assistant to help me organize this report; I ran and verified
every step myself and I understand what I'm reporting." Treat these
packages as AI-assisted work; the absence of AI is never the reading.
