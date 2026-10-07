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

|Diagnosis	| The plan's stated causal mechanism, read against the issue description and every piece of repro evidence (controls, traceback, regression window).|	The plan explains a specific causal mechanism — what code behavior produces the exact symptom reported — not a vague "something in the parser" or "a bug in validation." A precise function/method name counts as specific even without a file path, and vice versa; in a large codebase a named function is as locatable as a file:line. The mechanism must not contradict or ignore any repro evidence given (a control run that rules the named cause out, a regression window that points elsewhere). An explicitly flagged implementation-level uncertainty ("exact fix site may move one layer during implementation") does not fail this check as long as the causal mechanism itself is identified and grounded. Fails if the explanation is generic/hand-wavy with no mechanism, or if it contradicts or ignores repro evidence.	|required |
|Scope|	The plan's description of what it will change, compared against the Diagnosis mechanism above.|	The change area the plan names — a file path, a specific function, or a tightly described code path (not "the relevant module" or a whole directory with nothing narrower) — includes wherever Diagnosis pointed to, and the plan explicitly names at least one specific adjacent thing it will deliberately NOT do (a related symptom, file, or redesign it's leaving out — not a generic "won't break anything else"). Fails if the named area excludes or doesn't clearly include the Diagnosis mechanism's location, if the scope is only a module/directory with no narrower description, or if no explicit exclusion is stated.	|required|
|Test|	The plan's description of how it will confirm the fix, read against the repro evidence's steps and the issue's expected behavior.|	The plan names a concrete way to tell success from failure that is tied to the repro's actual trigger — an exact expected output, exit code, or described before/after behavior (ideally compared against a control) — whether via an automated test or a decisive, repeatable manual/scripted re-run of the repro. A named test file path is not required; a clearly described scenario is enough. Fails if no verification method is given, if it only checks unrelated happy-path behavior, or if success and failure can't be told apart from what's written ("test that it works").	|required|


## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
Verdict rule: unclear counts as fail for that check. Ready only if Diagnosis, Scope, and Test all pass — a fail (or unclear) on any one holds the plan at not-ready, regardless of how strong the others are.
