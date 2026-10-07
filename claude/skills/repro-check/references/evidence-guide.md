# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: in an eval bundle, the repro report's own environment line (usually near the
top of "Candidate repro report"), plus the issue's own stated "Environment:" line and the
repo-facts block's `latest release` line for what "current" means. In live mode, the
student's draft repro comment (its environment line), the issue thread (the reporter's own
environment line, if given), and the repo's latest release/tag on GitHub for comparison.

What good looks like: the tool's version and the OS are both named in the report, in the
same comment that carries the steps (not left to be inferred). If the issue is platform- or
driver-specific (e.g. a Windows-only bug, a GPU-driver bug), the relevant runtime/driver is
named too. If the version used differs from the version the issue targets, the report says
so in words ("issue filed against 0.63.1; reproduced here on 0.64.1") rather than leaving the
reader to notice the mismatch themselves.

## Steps

Where it lives: the repro report's steps section — a numbered list, a shell transcript, or an
inline description of the actions taken, between the environment line and the shown
artifact.

What good looks like: a stranger with the same repo and report, and nothing else, could
place themselves at the same starting point and reach the same trigger. A shell transcript
that starts from `git init` or a fresh checkout is followable; a report that says "in our
internal staging config" or references a file never shown or shared is not, no matter how
detailed the rest looks. On a platform-specific issue, the steps name the platform-specific
piece (a driver flag, a build profile) the issue itself calls out as load-bearing.

## Behavior shown

Where it lives: the artifact block in the repro report — command output, a log excerpt, a
screenshot description — read next to the issue's own description of the trigger and the
failure mode (what input, what error, what exit behavior).

What good looks like: the same operation and the same shape of input the issue names,
producing (or genuinely attempting to produce) the same class of failure the issue
describes. An artifact that used a different flag, a different argument shape, or an older
release's already-different behavior is not evidence about the reported bug, even if it
looks superficially similar (e.g. an exit-1 argument-validation error standing in for a
reported exit-101 crash). This is separate from whether the bug actually manifested: a
faithful attempt that comes back clean still counts as engaging the real trigger — what it
found is a matter for honesty, below, not for whether the right thing was tried.

## Honesty

Where it lives: the gap between the report's own words (its stated conclusion — "reproduced,"
"could not reproduce," "partially") and what the artifact block right above those words
actually contains.

What good looks like: the conclusion is exactly what the artifact supports. A "could not
reproduce" that shows the attempted steps, the resulting (non-matching) output, and names a
plausible reason (an environment difference, a config difference) is a complete, honest
report — this is a pass, not a lesser result. A conclusion is not honest when it asserts a
result ("verified," "confirmed the crash") with no artifact at all, or when it describes the
artifact as showing something it does not (a terminal still running described as a crash, a
normal completion described as the reported failure).

## Comms

Where it lives: the claim comment and the repro comment as their own text — the candidate
comments in an eval bundle, or the student's draft(s) in live mode — read against the
repo-facts block's contribution policy and any stated AI-disclosure requirement, and (live
mode only) the issue itself, to check the comment actually names it.

What good looks like: a comment reads as written for this specific issue — it could not be
pasted unchanged onto a different one. It states intent or findings plainly rather than in
template boilerplate ("+1," "assign me," a guaranteed fix by a specific date with no
investigation behind it). Where the repo's stated policy requires disclosing AI assistance,
at least one of the two comments says so in plain words; where the policy states no such
requirement (or is silent), no disclosure is needed to pass. A short, terse comment can
still be excellent here — specificity is the test, not length or template shape.
