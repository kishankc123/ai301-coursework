# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

kishankc123

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6032095578

Following up on my reproduction with a plan.

`PII_PATTERNS["phone_us"]` in `safety/pii_scrubber.py` allows an optional dash or dot at each separator position but not a space, which is exactly why `(555) 123-4567` doesn't match — the space after the closing parenthesis is what breaks it, not the parentheses themselves (confirmed by testing the pattern directly, and by closing the space to restore the match).

Plan: add a literal space alongside the dash/dot at all three separator positions in that one pattern, nothing else. Using a literal space rather than `\s`, since `\s` would also match newlines and could span unrelated lines in prose text.

Test: the issue's own repro snippet plus the four named tests (`test_us_phone_number_redaction`, `test_us_phone_formats`, `test_detect_phone_pii`, `test_phone_at_start_of_text`) flipping from `XFAIL` to passing, with the rest of the suite staying green.

Not touching in this change: the leading `(`/`+` stays unredacted after the fix (a pre-existing property of the pattern's word-boundary placement, not something this change introduces — it just never surfaced before since the space-separated case never matched at all), and `test_mixed_pii_and_text`'s `xfail` marker, whose actual failure is unrelated over-redaction of ordinary prose, not a phone number — a different defect for a different issue.

---

## Your branch

**Branch**

fix/53-redactfailfix

**Evidence**

**Before (posted in Unit 2):**

```
$ python -c "
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(repr(s.scrub('Call me at (555) 123-4567 or 555-123-4567')))
print(repr(s.detect('Call me at (555) 123-4567')))
"
'Call me at (555) 123-4567 or [REDACTED]'
[]
```

```
$ python -m pytest tests/unit/test_pii_scrubber.py -v -m unit
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction XFAIL
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats XFAIL
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii XFAIL
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text XFAIL
```

**After (same commands, re-run against the built change on `fix/53-redactfailfix`):**

```
$ python -c "
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(repr(s.scrub('Call me at (555) 123-4567 or 555-123-4567')))
print(repr(s.detect('Call me at (555) 123-4567')))
"
'Call me at ([REDACTED] or [REDACTED]'
[{'type': 'phone_us', 'value': '555) 123-4567', 'start': 12, 'end': 25}]
```

```
$ python -m pytest tests/unit/test_pii_scrubber.py -v -m unit
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text PASSED [ 72%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_end_of_text PASSED [ 76%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_mixed_pii_and_text XFAIL [ 92%]
======================== 24 passed, 1 xfailed in 0.23s =========================
```

Digits in the parenthesized number are now redacted and detected, matching the dashed
format's existing behavior; the leading `(` remains (a named, out-of-scope cosmetic
property of the pattern's word-boundary placement, not something this change introduces).
All four named tests flip from `XFAIL` to `PASS`; `test_mixed_pii_and_text` stays `XFAIL`
(its failure is a separate, mismarked defect, unrelated to this issue). Full-suite
regression check: `pytest tests/unit -m unit` → `379 passed, 49 xfailed`, no new failures.

## Eval iterations

**Run history**

Full run — 19/20 scored items (bar: 18/20 — PASS). Categories: clear-accept 7/7, scope-creep 4/4, thread-convention 1/2, unbuildable 3/3, wrong-cause 4/4. This was the first and only full run I committed: I built the rubric around the three failure families the lecture named that map most directly to what a plan needs before it can be built from (diagnosis contradicting evidence, unbounded scope, and an unverifiable test plan), ran the confirming full run directly with `--save-run eval-run.txt`, and got 19/20 with the category floor still met (at least one match in every category, including 1/2 in `thread-convention`).

**Package analysis**

`pkg-20` (source: a ghostty-org/ghostty issue, category `thread-convention`): my rubric's verdict is **accept**, and the gold label is **reject** — this is the one package my rubric disagreed with the gold label on. The candidate plan itself is excellent by every technical measure my rubric checks: Diagnosis names a precise mechanism (a stale `prev` pointer surviving a mid-print page reallocation) grounded in the repro's control run; Scope is one bounded change (a generation counter in `Terminal.print`'s grapheme paths) with an explicit exclusion (not recomputing `prev` unconditionally, the already-rejected expensive approach); Test names a concrete, repo-native verification (`zig build test` on both fuzz cases plus the control, plus a corpus re-run). All three of my required checks pass, so my rubric accepts it. The gold label rejects it for a reason none of my three checks look at: ghostty's stated `AI_POLICY.md` requires disclosing all AI usage in any comment, and the candidate plan comment never discloses it (the eval set treats every package as AI-assisted work for this purpose). My rubric has no check for upstream disclosure policy at all — it only judges whether the plan is technically ready to build from, not whether the comment complies with the repo's stated conventions around AI use. That gap is exactly why my rubric's `thread-convention` category landed at 1/2 instead of 2/2: it caught whatever the other `thread-convention` package tested, but has no mechanism to catch a disclosure-policy violation specifically.

**Check rationale**

The "Scope" check, as it now reads in `rubric.md`:

Evidence: "The plan's description of what it will change, compared against the Diagnosis mechanism above."

Pass condition: "The change area the plan names — a file path, a specific function, or a tightly described code path (not "the relevant module" or a whole directory with nothing narrower) — includes wherever Diagnosis pointed to, and the plan explicitly names at least one specific adjacent thing it will deliberately NOT do (a related symptom, file, or redesign it's leaving out — not a generic "won't break anything else"). Fails if the named area excludes or doesn't clearly include the Diagnosis mechanism's location, if the scope is only a module/directory with no narrower description, or if no explicit exclusion is stated."

It reads this way because the lecture named "the change is unbounded (scope creep)" as its own failure family, and a scope statement that only says what a plan *will* do can look bounded while still being open-ended in practice — the real test of boundedness is whether the plan can also name something specific it is choosing *not* to do, since a plan that cannot name an exclusion usually hasn't actually drawn a line yet. I required both halves (a narrow, Diagnosis-matching change area, and a named exclusion) rather than either alone, because a plan could satisfy a narrowness-only check with a tightly named file while still silently expanding into an adjacent refactor the moment it starts touching that file — the named-exclusion half is what catches that kind of expansion before the build starts, not after.

**Trade-offs**

My rubric has no check for AI-use disclosure, unlike my Unit 2 `repro-check` rubric, which does. I accept this gap for now rather than add a fourth check, because the eval set exercises it in exactly one package (`pkg-20`, the sole miss in my 19/20 run) and the category floor is still met on a technicality (1/2, not 0/2) because the set's other `thread-convention` package happens to test a different convention my checks do catch indirectly through Scope. The case I accept this will miss: any future plan that is technically excellent — precise diagnosis, bounded scope, verifiable test — on a repo whose stated policy requires AI disclosure, posted without disclosing it, will be wrongly accepted by my rubric exactly as `pkg-20` was. I did not re-run a canary after this analysis, since I'm not revising the rubric to close the gap this cycle (my confirming run already clears the 18/20 bar and the floor); the honest account of the trade-off, not a fix, is what this analysis is for.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
```
