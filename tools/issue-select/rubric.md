# Rubric: is this a good first issue?

Five required checks, one per way a first contribution dies: nobody is
home, the software is dead, the work is too big, somebody else already has
it, or my workflow is banned at the door. Three preferred checks rank the
issues that survive.

All recency thresholds are measured against the capture date stamped at the
top of the bundle (in live mode, against today).

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | Repo facts: the "last 5 default-branch commits" list (date + author per commit) and "last push to any branch", read against the capture date | At least one of the last 5 default-branch commits is dated within 90 days of the capture date AND is authored by a non-bot account (an author whose name ends in `[bot]` does not count on its own; a bot commit that merges a named human's PR does count) | required |
| project-in-use | Repo facts: the `archived:` flag on the repo line, "latest release", "last push to any branch" | `archived: no`, AND either a release published within 24 months of the capture date, or — if the repo has never published a release — a push to any branch within 90 days of the capture date | required |
| scope-one-change | The issue title and body, plus the whole comment thread | The issue asks for one bounded change that a single pull request could deliver, and none of the disqualifiers in "Scope disqualifiers" below is present | required |
| not-already-taken | Repo facts: "this issue: assignees:" and "linked PRs:" with each PR's state; plus every claim in the comment thread, with its date and any maintainer reply | All three hold: (1) assignees is none; (2) no linked PR is in state `open`; (3) no comment claiming the work ("I'll take this", "working on this", "@bot claim") is dated within 60 days of the capture date, and no maintainer has told other contributors to stand down. A claim older than 60 days with no PR merged since, a closed/unmerged PR, and a bot-unassigned claim are all abandoned, not live | required |
| ai-contributions-allowed | Repo facts: the "contribution policy" line (CONTRIBUTING.md, linked contributor docs, AI policy files, PR-template disclosures) | The policy does not refuse AI-assisted contributions: no statement banning AI-generated code or docs, and no statement that PRs made with generative AI get closed. Conditions — disclose, understand, test, human-review — pass, and so does silence (no CONTRIBUTING.md, or no statement on AI) | required |
| maintainer-responsive | Repo facts: "maintainer first-response sample", days to first owner/member/collaborator comment | At least 2 of the sampled issues got a maintainer comment, and the fastest of those was within 30 days | preferred |
| newcomer-signposted | The issue's labels, and the body's pointers to files, paths, or acceptance criteria | The issue carries a `good first issue` / `help wanted` / `easy` label, or the body names the files or directories to touch or lists acceptance criteria | preferred |
| issue-still-warm | The issue's opened date, the date of the newest comment, and the capture date | The issue was opened, or last commented on, within 12 months of the capture date | preferred |

### Scope disqualifiers

`scope-one-change` fails if any of these is present:

1. **Umbrella or tracking issue.** The body is mostly a list of other issue
   numbers or of sub-tasks meant to be split into separate pull requests.
2. **Standing invitation.** The issue asks for ongoing, open-ended
   contributions rather than one change ("PRs welcome, big and small",
   "incrementally adding more X"), so there is no state in which it is done.
3. **Core internals.** A maintainer says in the thread that the fix
   requires changes to core internals, or that a redesign has to land
   first.
4. **Unendorsed feature.** The issue proposes a new feature or enhancement
   (not a bug or a docs fix) and no maintainer has endorsed it: no
   `good first issue` / `help wanted` / `accepted` label, no
   OWNER/MEMBER/COLLABORATOR comment agreeing it should be built, and the
   opener is not one of them. Bug reports and doc fixes are exempt: a repo
   wants its bugs fixed whether or not anyone has said so in the thread.
5. **Repeatedly attempted and dropped.** Two or more linked PRs are closed
   unmerged, or three or more different people have claimed it in the
   thread and nothing has landed. However friendly the label, an issue that
   has thrown off that many attempts is harder than it looks.
6. **Not a contribution at all.** The issue is a usage or support question
   ("how do I get this to work?"), or the thread ends on a design question
   that a maintainer has not answered.

Two things that are *not* disqualifiers:

- **A terse body.** Judge the size of the work being asked for, not the
  polish of the write-up: a one-line bug report from a collaborator can be
  a perfectly bounded first issue.
- **Detail, length, or several files.** One coherent task is one change
  even when it is spelled out at length or spans several files in the same
  area (a set of documentation pages, four package files). Likewise, a
  diagnosis or a list of suggested optimizations inside a bug report
  describes how one defect might be fixed; it is not a demand for several
  separate pieces of work.

## Verdict rule

**accept** if and only if every `required` check grades `pass`. Any
required check graded `fail` or `unclear` produces **reject**: a first
issue whose safety I cannot verify from the evidence in front of me is not
a first issue worth taking, so `unclear` counts as `fail`.

`preferred` checks never change the verdict. They are reported with their
grades and used only to rank the accepted issues against each other, most
preferred passes first; ties broken by the fit profile in `scope.md`.
