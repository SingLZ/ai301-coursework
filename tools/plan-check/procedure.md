# Procedure: how this skill grades a plan package

These are steps for grading a plan, not for writing one. Follow them in
order. Where a step names a section, it means the section with that
heading in the package (eval) or the place the evidence guide names
(live).

## Read order

1. Read `rubric.md` and `references/evidence-guide.md`. Write down the
   seven check names, which are required, and the verdict rule.
2. Read the Repo facts block. Note two things: the `contribution
   policy` line word for word, and whether it requires AI disclosure in
   comments.
3. Read the Issue section. Note in one sentence the failure the
   reporter saw and the trigger that produced it.
4. Read the Thread highlights. For each entry, note the author's role.
   List separately every entry from an OWNER, MEMBER, COLLABORATOR, or
   CONTRIBUTOR that isolates a culprit, proposes or rejects an
   approach, posts a patch, or asks for something. Mark entries from
   NONE-role users as claims, not direction.
5. Read the Repro evidence block before the plan. Write down each step
   and control run with its result, then the Expected and Actual lines.
   Then write one sentence: "the evidence says the failure tracks ___
   and does not depend on ___." This sentence is fixed before you read
   the plan, so the plan's own story cannot shape what you think the
   evidence shows.
6. Read the Candidate plan, then the Candidate plan comment.

## Evidence gathering

1. Cause: copy the plan's stated cause (its Diagnosis or Cause line)
   word for word. If the plan comment asserts a cause too, copy it as
   well.
2. Cause against repro: for each repro step and control from Read
   order step 5, write whether the plan's cause predicts that result
   (yes / no). Note where the cause came from: the repro, or a thread
   comment.
3. Scope: list every change the plan proposes (from Scope, Changes,
   Approach, and any "also" or "while I'm here" sentence). Next to
   each, write which part of the reproduced failure it fixes or tests,
   or "none". Copy the not-in-scope line if there is one.
4. Executability: copy the files, functions, or sites the plan names.
   Copy every phrase that leaves a decision open ("somewhere", "or",
   "not sure", "investigate", "profile first", "whichever").
5. Test: copy the test plan. Write what observable result it names,
   and whether that result would differ between unfixed and fixed
   code, using the repro's Actual line as the unfixed result.
6. Thread direction: from the list made in Read order step 4, write
   for each maintainer direction whether the plan comment follows it,
   names it, or ignores it.
7. AI disclosure: if the policy requires disclosure, copy the sentence
   in the plan comment that discloses AI use, or write "none".
8. Unknowns: copy the plan's risks or unknowns lines, or write "none".
9. Live mode only: the repro evidence is the student's posted repro
   comment on the issue, or the repro text quoted in the drafts. The
   thread highlights are the live issue's comments by maintainers. The
   repo facts come from the repo's CONTRIBUTING and AI policy files.
   If none exist, the policy asks for no disclosure.

## Check execution

1. Grade the checks in rubric order: diagnosis-grounded,
   scope-bounded, executable, test-decisive, thread-direction,
   ai-disclosure, unknowns-stated.
2. For each check, compare only the notes gathered for it against the
   pass condition in `rubric.md`. Grade `pass`, `fail`, or `unclear`,
   and write the one fact or quote that decided it.
3. diagnosis-grounded fails if any "no" appears in Evidence gathering
   step 2. It also fails if the cause's only source is a thread
   comment.
4. scope-bounded fails if any listed change has "none" next to it and
   is not documentation of the fixed behavior or a deferral.
5. Grade `unclear` only when the package truly lacks the section the
   check reads (no test plan at all, no stated cause at all), never
   because you skipped it. An absent section that a check requires is
   graded `unclear`, which the verdict rule counts as a fail.
6. Do not let one check's result change another. A strong test plan
   does not rescue a wrong cause; a wrong cause does not fail scope.
7. Grade the substance, not the length. Do not fail a check because
   the plan is short or lacks headings, and do not pass one because it
   is long and confident.

## Verdict assembly

1. Take the six required checks. If all six are `pass`, the verdict is
   `accept`.
2. If any required check is `fail` or `unclear`, the verdict is
   `reject`.
3. Ignore `unknowns-stated` for the verdict; report its grade only.
4. In the summary before the JSON, name each check that caused a
   reject and quote the evidence that decided it.
5. Emit the JSON block exactly as SKILL.md specifies, with all seven
   checks, as the last thing in the output.
6. Live mode only: after the JSON verdict is decided, but before
   emitting it, hold the plan comment against `voice-guide.md` and list
   any broken rule in the summary. This never changes the verdict.
