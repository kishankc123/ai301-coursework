# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

kishankc123

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-5903493958

Hi! I'd like to work on this as a course exercise — this is my first time contributing to this repo, and I know several classmates are also on this issue, so I'm not assuming exclusivity, just posting my own claim and my own work.

I'll reproduce the two behaviors the issue describes on a clean checkout of `main`: `scrub()` leaving `(555) 123-4567` unredacted while it redacts `555-123-4567` in the same string, and `detect()` returning `[]` for the parenthesized format, plus the four named tests (`test_us_phone_number_redaction`, `test_us_phone_formats`, `test_detect_phone_pii`, `test_phone_at_start_of_text`) in `tests/unit/test_pii_scrubber.py`. I'll post a reproduction report here with my environment, the exact commands I ran, and what I observed, before looking at any fix.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-5903747356

## Reproduction report

Reproduced as described.

**Environment:** macOS 15.7.7 (arm64, Apple Silicon), Python 3.14.5, my fork of `codepath/pathreview-ai301-fa26-s1` at commit `f89c06f` (`main`, clean tree).

**Setup deviation:** I did not run the documented `make setup` path (Docker, Postgres, Redis, `alembic upgrade head`, frontend install). `safety/pii_scrubber.py` is pure regex with no DB or API dependency, so I created a venv and installed the dev extra directly, then ran only this module's tests:

```
$ python3 -m venv .venv
$ source .venv/bin/activate
$ pip install -e ".[dev]"
```

**The four named tests:**

```
$ python -m pytest tests/unit/test_pii_scrubber.py -v -m unit
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction XFAIL
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats XFAIL
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii XFAIL
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text XFAIL
======================== 20 passed, 5 xfailed in 1.14s =========================
```

All four `xfail`, matching the issue.

**Observed — the issue's own snippet, run verbatim:**

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

Matches the issue exactly: in the same string the dashed number is redacted and the parenthesized one is not, and `detect()` returns `[]` for the parenthesized format.

**Control — dashed format alone, same build:**

```
$ python -c "
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(repr(s.scrub('Call me at 555-123-4567')))
print(repr(s.detect('Call me at 555-123-4567')))
"
'Call me at [REDACTED]'
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
```

The control redacts and detects normally, so phone redaction works in general and the failure is specific to the parenthesized format, not to the feature as a whole.

**Expected:** `scrub()` redacts `(555) 123-4567` the same way it redacts `555-123-4567`, and `detect()` reports it.

**Actual:** reproduced as described — the parenthesized format passes through `scrub()` unredacted and `detect()` finds nothing for it, while the dashed format is handled correctly, on `f89c06f` in the environment above.

I have not diagnosed the cause in the pattern or proposed a fix; this report covers the reproduction only.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Full run — 20/20 scored items (bar: 18/20 — PASS). Categories: clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. This was the first and only full run: I hand-graded the warm-up package `calib-02` against my rubric first (reject — no environment, no steps, no artifact, pure "+1" boilerplate — matching the gold label), which confirmed the rubric's wording before spending credit on a full run, then ran the confirming full run directly with `--save-run eval-run.txt`, which produced the 20/20 result above with no further iteration needed.

**Package analysis**

`pkg-20` (source: `ghostty` issue, category `disclosure`): my rubric's verdict is **reject**, and the gold label is also **reject**. The package's repro work is excellent on every proof check — environment recorded, steps followable, faithful trigger, honest conclusion — but ghostty's stated contribution policy requires disclosing AI assistance, and neither the claim comment nor the repro comment in the package discloses it. My rubric's dedicated "AI-use disclosure" check reads the repo-facts block's policy line first, and only then checks whether either comment discloses; here the policy says "required" and the comments say nothing, so that one check fails and (per my verdict rule, where any required check failing holds the package at reject) the whole package rejects regardless of how strong the other five checks graded. This is the one-package `disclosure` category the eval set's category floor exists to force, and it's exactly the scenario I wrote the check to catch: a rubric that only judged reproduction quality would have accepted this package on its technical merits and missed the compliance failure entirely.

**Check rationale**

The "AI-use disclosure" check, as it now reads in `rubric.md`:

Evidence: "The repo-facts block's stated contribution policy (does it require disclosing AI assistance?), read against whether the claim comment or repro comment discloses AI assistance."

Pass condition: "Passes automatically if the repo's stated policy carries no AI-disclosure requirement. When the repo's stated policy does require disclosure, passes only if a comment discloses AI assistance; course packages are treated as AI-assisted work for this check, so silence where disclosure is required is a fail regardless of how strong the rest of the package is."

It reads this way because the lecture's proof families named "the words respect the repo's conventions" as its own category, separate from reproduction quality, and the eval set backs that with a category (`disclosure`) that exists specifically to test whether a rubric treats it as a standalone, required gate rather than folding it into a general "comms are good" check. I wrote it as its own row instead of merging it into "Comms are specific" so that a technically flawless, well-written package (like `pkg-20`) still cannot buy its way past a missing disclosure — the two things measure different failures (specificity of the words vs. compliance with a stated policy) and conflating them would let a strong report mask a compliance gap.

**Trade-offs**

The "Faithful trigger" check's pass condition explicitly does not require the bug to have manifested — a faithful attempt at the same trigger that comes back clean still passes that check, with honesty about the outcome graded separately by "Honest conclusion." I accepted this trade-off deliberately, because the gold-label set treats an evidenced cannot-reproduce (`pkg-09`, `pkg-10`) as a full accept, not a partial one, and a rubric that required the bug to actually reproduce before passing "Faithful trigger" would have rejected both of those clear-accept packages. The risk I accept in exchange: a package could faithfully attempt the named trigger, fail to reproduce it because of a real but unacknowledged version or platform difference, and still pass "Faithful trigger" as long as it's honest that it didn't reproduce — my rubric would accept that as a legitimate cannot-reproduce rather than flagging the possible environment mismatch as the real cause. I checked this doesn't let anything through in the scored set: `pkg-16`, the one package built around exactly this shape (an old pandas version standing in for current behavior), still correctly rejects — but there because its "Environment recorded" check fails on the silent version mismatch, not because "Faithful trigger" caught it. That's the trade-off in practice: two checks share the work of catching a version-driven false negative, and I rely on "Environment recorded" to do it rather than asking "Faithful trigger" to also police it.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
