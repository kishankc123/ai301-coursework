# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|

|Diagnosis	| The plan's stated root cause, read against the actual code at the file/line(s) named in the issue description.|	The plan names a specific file and function/line as the cause (not "something in the parser" or "a bug in validation"), and that location is one of the files actually implicated by the issue's repro/traceback. Fails if the plan jumps straight to a fix with no causal explanation, or if the named cause isn't in a file the issue actually touches.	|required |
|Scope|	The plan's list of files/functions it will change, compared against the diagnosis above.|	The plan names every file it will modify (not "the relevant module"), includes the exact file identified in Diagnosis, and states at least one adjacent file or area it will deliberately NOT touch (e.g. "will not modify the public API in client.py"). Fails if the root-cause file from Diagnosis is missing from the change list, or if "won't touch" is absent or generic ("won't break anything else").	|required|
|Test|	The plan's description of the automated test(s) it will add, read against the Diagnosis and Scope file list.|	The plan specifies a test file path and names the function/scenario being exercised, and that test imports or exercises the same file(s) named in Scope — not a new file unrelated to the bug. Fails if no test is mentioned, if the test only checks happy-path behavior unrelated to the bug, or if it targets different code than the diagnosed cause.	|required|


## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
Verdict rule: unclear counts as fail for that check. Ready only if Diagnosis, Scope, and Test all pass — a fail (or unclear) on any one holds the plan at not-ready, regardless of how strong the others are.
