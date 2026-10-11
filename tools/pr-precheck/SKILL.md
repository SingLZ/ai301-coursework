---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: **is this ready to
submit?** A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence) read against three
things: the plan it claims to implement (with any deviation notes),
the issue that plan belongs to (its thread included), and the repo's
stated standards (its PR template, contributing guide, and any AI-use
policy).

Do not answer any other question. Do not judge whether the issue was
worth taking, whether the plan was a good plan, or whether you would
have fixed it differently: the plan was already accepted, and you grade
whether the PR delivers it honestly, with proof, in a reviewable diff,
meeting the repo's asks. Grade exactly one package per run, and never
answer from gut feel. Execute the components in this directory.

## Inputs and modes

Work out which mode you are in before reading anything else. If the
prompt hands you an eval package bundle, you are in eval mode.
Otherwise you are in live mode.

**Live mode** (the student's own PR, before it is opened). Read these
inputs, from these sources, and nothing else:

- **Plan**: `plan.md` in the working copy's top folder, including its
  `## Deviations` section. The plan's Scope, Files, Approach, and Test
  plan are what the diff and the evidence are read against.
- **Diff**: run `git diff main...HEAD` (three dots) from the working
  copy, and `git log --oneline main..HEAD` for the commit list. This is
  every committed change the branch makes. Uncommitted or untracked
  files are not part of the PR: do not grade them as diff, and do not
  count `plan.md`, `comment.md`, `pr_draft.md`, or `test_evidence.md` as
  changes.
- **Title and description**: `pr_draft.md`. Its first line is the
  title, and the rest is the description.
- **Test evidence**: `test_evidence.md`, plus whatever before/after and
  check results the description itself pastes in.
- **Issue**: the issue URL in the prompt. Read its body and comment
  thread with `gh issue view <number> --repo <repo> --comments` (or the
  GitHub API or web page if `gh` is unavailable).
- **Repo standards**: `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` from the working copy (on `main`), and any AI
  policy file the repo has.
- **Branch name**: `git branch --show-current`, read against the house
  rules in `scope.md` and the naming convention in CONTRIBUTING.

A house-chain student (one routed to the instructor's house issue)
reads the house plan and the house repro pack in place of their own
plan and repro. The same checks grade the same things. If a file the
list names is missing, do not hunt for a substitute elsewhere in the
working copy: the checks that read it grade on its absence.

**Eval mode** (an eval package bundle). The bundle is the whole world.
Every fact comes from the bundle text: its repo-facts block, issue,
thread highlights, plan context (the plan plus the repro evidence it
built on), and candidate PR (title, description, commits, diff, test
evidence). Fetch nothing, run no commands, and read no other file.
Always grade a complete package, every check, under the full verdict
rule. Read every package as AI-assisted work.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. If its `Repo:`
line still holds the bracketed placeholder (`<ORG>/<PATH-REVIEW-REPO>`
or anything in angle brackets), stop without grading. Tell the student
to fill the `Repo:` line in `scope.md` with their section's Path Review
repo, and emit nothing else: no checks, no JSON verdict. Never guess a
scope.

Once the repo line is filled, check that the issue URL and the working
copy's `upstream` (or `origin`'s parent) belong to that repo. If they
point at any other repository, refuse to grade, and say which repo the
scope allows. Then apply the house rules `scope.md` lists as part of
the evidence: the `fix/<issue-number>-<slug>` branch from the student's
fork, one PR per issue, every template section filled with real
content. A classmate's PR on the same issue is never a reason to hold.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the verdict is decided and before you emit it,
read `voice-guide.md` and hold the outgoing words, the PR title and
description in `pr_draft.md`, against each rule in it. In the summary,
list every rule the draft breaks. Quote the rule and the line that
breaks it, and suggest a rewrite. Voice notes never change a check
grade or the verdict, because no rubric check reads the voice guide:
voice is personal and has no gold label.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the four judgment files in this order, and use each for its own
job:

1. `rubric.md` is the brain. Its table defines every check (name,
   evidence, pass condition, weight), and its verdict rule says how
   grades become accept or reject. Grade every check it lists, by
   exactly the names it uses, and no check it does not list.
2. `references/evidence-guide.md` is the map. For each evidence family
   it says where the evidence lives (in a bundle and in live mode) and
   what good looks like. When a check names evidence, look where the
   guide says.
3. `procedure.md` is the hands. Execute it as written, in order: its
   read order, its evidence gathering, its check execution, and its
   verdict assembly.
4. `voice-guide.md` (live only), as the voice seam above describes.

If the procedure is silent on a step you need, do not improvise around
it. Take the narrowest reading the rubric supports, and name the gap in
the summary as "procedure gap: ...", so the student can fix it.

If `rubric.md` has no filled check rows, or `procedure.md` has no
written steps under its headings, refuse to grade. Say which file is
empty and that this tool cannot grade without both a rubric and a
procedure, by design. Do not invent checks at runtime.

## Verdict and output

The verdict is binary. `accept` means ready to submit: open the PR as
it stands. `reject` means hold: fix what the failing checks name
first. There is no third verdict, no "accept with reservations", and
no score. Reservations go in check evidence lines.

Before the JSON block, write a short readable summary: one line per
check with its grade and deciding fact, then the deciding check for a
reject, then any procedure gaps, then (live only) the voice notes. End
your reply with this fenced JSON block, with one entry per rubric check
in rubric order, valid, and last. Nothing may follow it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

For `item`, use the bundle id in eval mode. In live mode, use the issue
URL followed by the branch name, since the PR does not exist yet.

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it: a file name from the diff, a line from the
  plan, a command and its output. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse PR whose diff matches
  the plan and whose evidence shows the repro flip is ready. A long,
  confident description gets no credit for confidence: read the diff
  and the evidence themselves, never the description's claims about
  them.
- **The rubric decides, not the run.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric.
- **The procedure decides how, not the run.** Follow `procedure.md`
  as written and report its gaps instead of inventing steps.
- **Honest shortfalls are not failures.** Something the plan
  deliberately deferred, or a deviation recorded in the plan's notes
  and restated in the description, is honest work and can be accepted.
  Only a gap nobody disclosed fails.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs. Where the rule is silent, an unverifiable claim
  is a failing one: a PR you cannot verify from the package is not
  ready to submit.
