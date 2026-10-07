# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: in an eval bundle, the candidate plan's own stated cause — usually a short paragraph or bullet near the top, naming a file/function and explaining why it produces the reported behavior — read against the issue context (the reported symptom, traceback, or repro steps) and the repro-evidence block (the file/line the reproduction actually pins the failure to). In live mode, the draft plan comment's diagnosis, read against the issue thread (the original report, any maintainer reply narrowing the cause) and the student's own posted repro comment.

What good looks like: the stated cause names a specific mechanism — a function, a code path, or a file — that accounts for the exact symptom in the issue, and that mechanism does not contradict or ignore any repro evidence given (a control run that behaves differently, a regression window, a traceback). A file path is not required if a function or method name is precise enough to locate in the codebase (e.g. `AdaptDispatch::EraseInDisplay` is as locatable as a file:line in a large project), and many issues give no file at all — in that case, the plan's own causal explanation carries the weight, so it must be mechanism-level ("the command is spawned with the filename in option position") rather than hand-wavy ("something in the parser"). An explicitly named implementation-level unknown ("exact clamp site may move during implementation") does not make a diagnosis ungrounded as long as the mechanism itself is identified. A diagnosis is ungrounded when it names no mechanism at all, when it explains a different symptom than the one reported, or when a control run or regression window in the repro evidence rules out the named cause and the plan doesn't address that conflict.

## Scope

Where it lives: the plan's own in-scope statement (its file list or "files changed" section) and any explicit not-in-scope line, read against the Diagnosis's root-cause file and against the repo-facts block or repo structure for what else lives near that file.

What good looks like: wherever Diagnosis pointed to — a file, or a named function/code path — is clearly inside the in-scope description; a plan that diagnoses one place but proposes changing a different one is not bounded, it's misdirected. The in-scope description itself can be a file path, a small named set of files, or a specific function/branch ("the erase-scrollback branch of `EraseInDisplay`") — it does not have to enumerate every touched file by path as long as the described area is narrow and locatable, not a whole module or directory named with nothing narrower. A bounded plan also names, in words, something adjacent it will deliberately leave alone ("will not touch the public API in `client.py`," "the mouse-selection symptom noted in the thread, filed separately") — generic reassurance ("won't break anything else") does not count. A plan with no in-scope description at all, or one that spans multiple unrelated subsystems with no stated exclusion, reads like a drive-by rewrite, not a bounded change.

## Executability

Where it lives: the plan's description of the actual work — the approach or steps section, usually below the scope statement — read as instructions a stranger with only the repo and the plan would follow.

What good looks like: someone who has not seen the issue could open the named file(s) and start making the described change without first messaging the author to ask what "update the validation logic" means. The plan names the specific function or code path to change and the shape of the change (add a check, move a call, fix a condition) rather than only the outcome desired. A plan that describes only the end state ("fix the race condition") with no path to get there is not executable, no matter how correct the diagnosis is.

## Test plan

Where it lives: the plan's test section — a named test file and a described scenario or assertion — read against the repro-evidence block's steps and artifact (what input triggered the bug, what output/error confirmed it) and against the Scope file list.

What good looks like: the described check maps onto the same trigger the repro evidence used (same input shape, same failure mode) — not a new happy-path test unrelated to the reported bug — and names an exact, observable outcome (an exit code, a specific printed value, a described before/after behavior, ideally against a control) that would fail today and pass after the fix. This does not have to be an automated test with a named file path: a decisive, repeatable manual or scripted re-run of the repro ("5 consecutive SSH reattach cycles with no rgb strings in any pane; fresh-create still clean") is just as good as a named unit test, as long as the pass/fail line is exact. A vague plan says only "add a test for this" or "make sure it works" with no scenario or expected outcome named, automated or not.

## Honesty

Where it lives: any risks, unknowns, or open questions section in the plan, and the gap between how confidently the plan states its diagnosis/fix versus what the repro evidence actually established.

What good looks like: the plan distinguishes what it has confirmed (via the repro evidence) from what it is assuming (an untested edge case, a related code path it hasn't checked). Stated unknowns ("this may also affect the async path, not verified") are a pass, not a weakness. False confidence looks like a plan that asserts the fix is complete or risk-free with no acknowledgment that the diagnosis rests on a single repro case, or that glosses over a repro evidence result that only partially matched the issue.

## Comms

Where it lives: the plan comment's own text — the candidate comment in an eval bundle, or the student's draft in live mode — read against the issue thread's maintainer signals (any stated preference for how fixes should be proposed, linked contributing docs) and the repo-facts block's contribution policy, including any AI-use disclosure requirement.

What good looks like: the comment is specific to this issue and this plan — it names the diagnosis and the intended file(s) in plain language, not generic "I'll submit a PR for this" boilerplate that could sit on any issue. Where the repo's stated policy requires disclosing AI assistance, the comment says so in words; where the policy is silent or states no such requirement, no disclosure is needed to pass. Thread-aware means the comment responds to anything a maintainer already said (a requested approach, a rejected prior attempt) rather than ignoring it.