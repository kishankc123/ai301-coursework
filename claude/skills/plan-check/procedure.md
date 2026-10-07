# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->
Read the issue description first, in full, before opening the plan. Note: the file(s)/function(s) it names, the exact failure behavior described (error message, wrong output, crash), and any reproduction steps or traceback included.
Read the repro evidence (if the package includes one) second. Note: which file/line it pins the failure to, and whether that location matches what the issue description implies. This is the independent check on "what's actually broken" — reading it before the plan prevents the plan's own framing of the bug from becoming the only source of truth.
Read the plan last, straight through once without grading, to see its full shape (diagnosis, file list, test description) before scoring any individual check. Note any file or function the plan names that did not appear in steps 1–2 — that's a mismatch to check later, not evidence to discard.

Order matters because Diagnosis and Scope are graded against the issue/repro facts, not against the plan's own claims about itself — reading the plan first would let a confident-sounding but wrong diagnosis set the frame.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

For each rubric check, pull and record the following before grading:

Diagnosis: From the plan, record the exact file + function/line it names as root cause, and its stated explanation of why that causes the bug. From step 1/2 notes, record the file(s)/line(s) the issue and repro evidence actually implicate. These two go side by side — same file, or not.
Scope: From the plan, record the full list of files/functions it says it will change, plus any explicit "will not touch" statement. Compare the file list against the Diagnosis root-cause file recorded above — is it in the list?
Test: From the plan, record the named test file path and the scenario/function it says the test exercises. Compare against the Scope file list recorded above — does the test touch the same files, or something unrelated?

No check's evidence should require re-opening the issue or repro from scratch at grading time — if a comparison needs a fact not captured here, go back and record it now, not during Check execution.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

Grade in this order: Diagnosis, then Scope, then Test — each depends on the previous one's recorded file(s), so grading out of order means re-deriving facts you already have.

For each check, compare the recorded evidence against the rubric's pass condition and assign one of three grades:

P (pass): the evidence fully satisfies the pass condition as written.
F (fail): the evidence contradicts the pass condition, or is entirely absent (e.g., no root cause stated, no test mentioned, no file list given).
? (unclear): some evidence exists but doesn't fully resolve the condition — e.g., the plan names a file but not a function/line, or says "will add a test" without naming a file or scenario. Only use ? when partial evidence is actually present; a check with nothing written for it is F, not ?.

Do not grade a check as P on the strength of confident language alone — the comparison must be against the recorded facts from Evidence gathering, not the plan's own self-assessment ("this definitely fixes the root cause").

A check may be graded without re-reading the full package once its two sides (plan claim vs. issue/repro fact, or plan claim vs. prior check's recorded file) are both already captured in your Evidence gathering notes.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

Apply the rubric's verdict rule: ? counts as F for that check. The plan is ready only if Diagnosis, Scope, and Test all grade P. Any single F (including a downgraded ?) holds the plan at not ready, regardless of the other two checks.

In the output, quote the specific recorded evidence for whichever check decided the verdict (the first F/? encountered in Diagnosis → Scope → Test order) — e.g., the plan's file list next to the Diagnosis root-cause file, if Scope is what failed — so the verdict is traceable to a concrete mismatch, not a summary judgment.