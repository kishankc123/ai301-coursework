# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: in an eval bundle, the candidate plan's own stated cause — usually a short paragraph or bullet near the top, naming a file/function and explaining why it produces the reported behavior — read against the issue context (the reported symptom, traceback, or repro steps) and the repro-evidence block (the file/line the reproduction actually pins the failure to). In live mode, the draft plan comment's diagnosis, read against the issue thread (the original report, any maintainer reply narrowing the cause) and the student's own posted repro comment.

What good looks like: the stated cause cites the same file/function the repro evidence actually touched, and the plan's explanation of "why this causes that" accounts for the specific symptom in the issue — not a generic plausible-sounding bug in the area. A diagnosis is ungrounded when it names a file the repro evidence never exercises, when it explains a different symptom than the one reported, or when it asserts a cause with no reference to the repro evidence at all (reasoning from the issue title alone).

## Scope

Where it lives: the plan's own in-scope statement (its file list or "files changed" section) and any explicit not-in-scope line, read against the Diagnosis's root-cause file and against the repo-facts block or repo structure for what else lives near that file.

What good looks like: the file(s) that Diagnosis named as the root cause appear in the in-scope list — a plan that diagnoses one file but proposes changing a different one is not bounded, it's misdirected. A bounded plan also names, in words, something adjacent it will deliberately leave alone ("will not touch the public API in `client.py`," "will not update the CLI help text"). A plan that lists no files, or whose scope is a module/directory name rather than specific files, reads like a drive-by rewrite waiting to happen, not a bounded change.

## Executability

Where it lives: the plan's description of the actual work — the approach or steps section, usually below the scope statement — read as instructions a stranger with only the repo and the plan would follow.

What good looks like: someone who has not seen the issue could open the named file(s) and start making the described change without first messaging the author to ask what "update the validation logic" means. The plan names the specific function or code path to change and the shape of the change (add a check, move a call, fix a condition) rather than only the outcome desired. A plan that describes only the end state ("fix the race condition") with no path to get there is not executable, no matter how correct the diagnosis is.

## Test plan

Where it lives: the plan's test section — a named test file and a described scenario or assertion — read against the repro-evidence block's steps and artifact (what input triggered the bug, what output/error confirmed it) and against the Scope file list.

What good looks like: the named test touches the same file(s) listed in Scope, and the scenario it describes maps onto the same trigger the repro evidence used (same input shape, same failure mode) — not a new happy-path test unrelated to the reported bug. A decisive test plan names an assertion that would fail today and pass after the fix; a vague one says only "add a test for this" with no file, scenario, or expected assertion named.

## Honesty

Where it lives: any risks, unknowns, or open questions section in the plan, and the gap between how confidently the plan states its diagnosis/fix versus what the repro evidence actually established.

What good looks like: the plan distinguishes what it has confirmed (via the repro evidence) from what it is assuming (an untested edge case, a related code path it hasn't checked). Stated unknowns ("this may also affect the async path, not verified") are a pass, not a weakness. False confidence looks like a plan that asserts the fix is complete or risk-free with no acknowledgment that the diagnosis rests on a single repro case, or that glosses over a repro evidence result that only partially matched the issue.

## Comms

Where it lives: the plan comment's own text — the candidate comment in an eval bundle, or the student's draft in live mode — read against the issue thread's maintainer signals (any stated preference for how fixes should be proposed, linked contributing docs) and the repo-facts block's contribution policy, including any AI-use disclosure requirement.

What good looks like: the comment is specific to this issue and this plan — it names the diagnosis and the intended file(s) in plain language, not generic "I'll submit a PR for this" boilerplate that could sit on any issue. Where the repo's stated policy requires disclosing AI assistance, the comment says so in words; where the policy is silent or states no such requirement, no disclosure is needed to pass. Thread-aware means the comment responds to anything a maintainer already said (a requested approach, a rejected prior attempt) rather than ignoring it.