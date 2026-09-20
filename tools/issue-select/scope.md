# Scope: where to look, and who is looking

<!--
This file is the skill's field of view. The rubric (rubric.md) decides
whether an issue is GOOD; the scope decides which issues are candidates
at all, and whose hands the issue would land in. It applies in live mode
only: in eval mode the bundle is the whole world and this file is
ignored.

Two parts. Staff wrote the first; you write the second.
-->

## Where candidates come from

Only issues in the course's Path Review repository are candidates:

- Repo: `codepath/pathreview-ai301-fa26-s3` <!-- paste your section's repo from the Unit 1 Check-In page -->

Do not search, fetch, or grade issues from any other repository, however
promising. The wider GitHub comes later in the course; for now the field
is Path Review.

**Path Review house rule.** Path Review is a classroom, and your
classmates are not strangers. Ignore the usual claim signals here: other
students' claim comments (and there may be several on one issue) do not
block an issue, and finding some on the issue you want is normal. Claim
anyway: course credit attaches to the pull request you open, not to
whether it merges, so a shared issue costs nobody anything. Everything
else in the rubric applies as written.

## Your fit profile

I work mainly in **Python**: scripts, small command-line tools, and
backend code. I am comfortable in a terminal with git and a virtualenv, and
I read and write Markdown docs without trouble. I can follow HTML/CSS and
small pieces of JavaScript, but front-end work is something I want to get
better at rather than something I have shipped.

For a first contribution I want one of three things, in this order: a small
bug fix with clear reproduction steps, a documentation-only change, or a
small UI/UX fix in a web app. What I most want to practise is reading an
unfamiliar codebase well enough to make one narrow change, and following a
project's PR process end to end.

Rank accepted issues higher when they are Python, docs, or a contained
front-end fix; when the issue names the files to touch or gives acceptance
criteria; and when I can reproduce the problem locally without heavy setup.
Rank lower anything that needs containers, a database, or a long build
before I can see the bug, and anything whose fix is mostly performance
tuning of code I would have to learn first.
