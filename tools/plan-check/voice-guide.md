# Voice guide: how I talk upstream

<!-- Draft written with Claude from my rubric work; the rules are the
ones I keep breaking, but the wording is mine to keep editing. Live
mode reads this file before any comment of mine goes out; eval mode
ignores it. -->

## Who I am in threads

I am new to this repo and I say so once, plainly, without apologizing
for it. What I am doing here is narrow: I reproduce one issue, I post
what my machine actually printed, and I say what I plan to look at
next. What a maintainer can expect from me is that every sentence I
write is something I ran, and that if I could not reproduce it, I will
say that just as loudly as if I had.

## Rules I write by

### Rule: No dates, no guarantees

I never promise when something will be done or that it will be done at
all. I can say what I am going to do next; I cannot say when the fix
lands, because I do not know the code yet.

- Wrong: "Please assign this to me, I'll have a PR up within 2 days."
- Right: "I'd like to take this one. Next step for me is reading how
  the translator loads locale files relative to when FES fires; I'll
  report back what I find."

### Rule: Show the output or say nothing

If I claim a behavior, the thing the machine printed is in the comment.
If I have no artifact, I do not have a finding — I have an impression,
and impressions do not go upstream.

- Wrong: "Can confirm, it crashes for me too."
- Right: "Reproduced on 3.2.4; the request goes out without
  `Content-Type: application/json`:" followed by the pasted
  `--offline` transcript and the control run without the custom header.

### Rule: Name the gap instead of smoothing it over

When my environment, version, or result differs from the issue's, I put
the difference in the comment in the same breath as the result. The
gap is information for the maintainer; hiding it makes my whole report
untrustworthy.

- Wrong: "Reproduced, same error as described."
- Right: "The issue was filed against 3.8.4; both shapes still
  reproduce on 3.9.6 (report below). My environment differs from the
  reporter's in shell — zsh, not fish."

### Rule: A failed attempt is a result, posted as one

If I could not reproduce it, I post the attempt anyway: what I ran,
what came out, and what I think differed. I do not quietly drop the
issue and I do not inflate a near-miss into a confirmation.

- Wrong: "Mostly reproduced — close enough, I think it's the same bug."
- Right: "I could not reproduce scenario 2 on my machine (attempt and
  artifacts below). In every run all ONE batches flushed before any
  TWO. I think a distribution where argument lengths differ per file
  is needed; I did not find a way to force a smaller limit from the
  CLI."

### Rule: Disclose the assistant where the repo asks

If the repo's policy asks for AI disclosure, I say which assistant I
used and for what, in the comment itself, and I say that I ran and
understand the work. I do not hope nobody asks.

- Wrong: (posting a polished report in a repo whose AI policy requires
  disclosure, saying nothing about how it was written)
- Right: "Per the AI usage policy: I used an AI assistant to help me
  organize this report; I ran and verified every step myself and I
  understand what I'm reporting."

### Rule: State the approach, and name what I don't know yet

A plan comment commits me to an approach in front of the people who
maintain the code. I say which file and which change, I tie it to my
repro, and I name the one thing I have not checked. If a maintainer or
classmate already suggested a direction, I say whether I'm following it.

- Wrong: "I traced the root cause and this fix will solve it."
- Right: "Plan: add an empty-corpus guard at the top of `index()` in
  `rag/retriever/keyword_search.py`, matching the one `search()`
  already has. Test: re-run my repro's `index([])`, which should
  return without raising. Not yet checked: whether any caller relies
  on `bm25` being set after an empty index."

## Things I never post

- Timelines, delivery promises, or "guaranteed" anything.
- "+1", "same here", "any update on this?" with nothing attached.
- A root cause I have not tested, stated as if I had tested it.
- Flattery as an opener ("Great project! I love this repo!") — it
  costs the maintainer a line and buys me nothing.
- A claim that *asserts* the bug reproduces before I have run it.
  Claiming first is fine, and is how I take an issue: the claim names
  the attempt I am about to make and promises the report. What it never
  does is report a result I do not have yet.
- The word "confirmed" over anything but an artifact that shows the
  issue's own failure.
