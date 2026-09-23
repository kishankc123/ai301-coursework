Unit 1 — Issue Selection

Path: beat-1-sandbox/unit-1/selection.md

Record of the issue carried into Unit 2, and of the evaluation runs that produced eval-run.txt. This file is graded at the path above; a copy kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong label is not graded.

Selected issue

Issue link

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53

Verdict output

Evidence gathered (live mode, measured against today 2026-09-22)

Repo facts — codepath/pathreview-ai301-fa26-s1, not archived, default branch main, last default-branch commit 2026-09-16T21:48:26Z by Aburke225 (COLLABORATOR). Only 1 PR has ever existed in the repo (#74, open, links #60). All three issues are inside the scoped repo.

#53 — PII scrubber fails to redact parenthesized US phone numbers

- Repo is active — pass. Same 2026-09-16 commit.
- Maintainer is alive — pass. Same evidence.
- Not already claimed — pass. assignees: [], 0 comments, no open linked PR. Two referenced timeline events (Evin009, ElvisValcarcel) are fork commits, not PRs — and under the Path Review house rule classmate activity does not block an issue anyway.
- Scope fits a newcomer — pass. Named file pii_scrubber.py, runnable repro, four named failing tests in tests/unit/test_pii_scrubber.py.

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53",
  "checks": [
    {"name": "Repo is active", "grade": "pass", "evidence": "Most recent default-branch commit 2026-09-16T21:48:26Z (Aburke225), 6 days before today 2026-09-22."},
    {"name": "Maintainer is alive", "grade": "pass", "evidence": "Default-branch commit 2026-09-16 is within 90 days; issue author Aburke225 has author_association COLLABORATOR."},
    {"name": "Not already claimed", "grade": "pass", "evidence": "assignees: [], 0 comments, no linked PR (repo's only PR #74 links #60); two 'referenced' events are fork commits, and the house rule discounts classmate claims."},
    {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Single named file pii_scrubber.py with runnable repro and four named failing tests; no closed PRs exist in the repo, so no stalled attempts."}
  ],
  "verdict": "accept"
}
Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

Run history

Full run (4 required checks: repo activity 30-day commit threshold; maintainer liveness via comments only, 90-day threshold; claim check treating any past claim comment or linked PR as blocking; scope check gated on a good-first-issue label or a named file/function/error) — 13/20 scored items (bar: 18/20 — below the bar). Categories: claimed 4/4, clear-accept 2/8, dead-repo 3/3, policy 1/1, scope 3/4.
Partial re-check (--only on the 7 disputed issues: issue-01, 06, 09, 11, 14, 15, 19) after broadening the maintainer-liveness check to count maintainer commits/merges as well as comments, allowing stale claims with no follow-up PR to pass the claim check, and rewriting the scope check to require a detailed, decision-free plan with no history of stalled attempts — 4/7 agree.
Partial re-check on the same 7 issues after simplifying the claim check to judge current PR/assignee status rather than claim history, and tightening the scope check's wording — 3/7 agree (a net regression: issue-11 flipped from correct to incorrect).
Partial re-check on the same 7 issues after rewriting the maintainer-liveness evidence to use commit recency for any author (dropping the unverifiable "was this commit author a maintainer" inference) and reframing the scope check as pass-by- default, reject-only-on-a-named-red-flag (tracking list, unresolved design decision, or stalled history) — 5/7 agree.
Partial re-check on the 2 remaining disputed issues (--only issue-09,issue-19) after clarifying that a terse-but-clear ask and multiple named causes for one diagnosed bug do not count as an unresolved decision — 1/2 agree (issue-19 fixed; issue-09 still disagreed).
Full run — 18/20 scored items (bar: 18/20 — PASS). Categories: claimed 4/4, clear-accept 6/8, dead-repo 3/3, policy 1/1, scope 4/4. Remaining misses: issue-01 and issue-09, both on the scope check.
[TODO — replace with the agreement line from the actual --save-run eval-run.txt run you commit. It should be close to 18/20 but may vary slightly, since grading is done by an LLM and isn't perfectly deterministic between runs — issue-01 itself flipped between a passing isolated re-check and this full run.]

Issue analysis

issue-09 (conda/conda#7617): my rubric's verdict is reject, failed on "Scope fits a newcomer." The gold label is accept.

The issue is a two-sentence, six-year-old feature request: add a conda config --clear flag that empties a list-type config entry, with one YAML example. My scope check reads this as underspecified — it never says how --clear should behave on a non-list config key, or what should happen on invalid input, and my check treats that missing detail as a sign of an unresolved design decision the maintainer hasn't made. The gold label treats it as bounded instead: the core behavior (empty the named list) is unambiguous even without those edge cases spelled out, and a newcomer could resolve them with a quick clarifying comment or by following the conventions of conda's existing config commands. This is a genuine threshold disagreement about how much implementation detail counts as "enough," not a case where my rubric missed an obvious signal — my check simply sets that bar a little higher than the gold label does.

Check rationale

The "Maintainer is alive" check, as currently written in rubric.md:

Evidence: repo-facts block: date of the most recent commit on the default branch (any author); comment thread: commenter roles (OWNER/MEMBER/COLLABORATOR) and comment dates

Pass condition: The most recent default-branch commit is within the last 90 days, OR a commenter with an OWNER, MEMBER, or COLLABORATOR role has commented on any issue or PR within the last 90 days

It's shaped this way because an earlier version required the model to determine whether the author of a recent commit held a maintainer role — but the repo-facts commit list in the evidence bundle never labels commit authors by role (only comment threads carry role tags like MEMBER or COLLABORATOR). Asking the model to infer an unlabeled attribute produced inconsistent grades on identical rubric text between runs — the same issue (issue-09) passed this check on one run and failed it on the next with no wording change in between. The current form only asks for things the evidence states directly: commit recency (regardless of author) as one path to a pass, and role-tagged commenter activity as the other.

Trade-offs

Accepting commit recency from any author, not just a confirmed maintainer, means a repo whose only recent activity is a routine dependency-bump commit (from a bot, or a one-off contributor) can pass this check even if the humans who'd review a newcomer's PR have gone quiet. I accepted that risk because the alternative — requiring provable maintainer authorship — was unanswerable from the evidence and made grading less stable, not more accurate. I re-ran --only issue-02,issue-07,issue-17 (the three dead-repo issues) after this change to confirm it didn't let a genuinely dead repo through the looser door: all three still correctly reject, since none has any commit or role-tagged comment within 90 days.

Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the reasoning is, and not on length — a short honest answer to each earns the full marks. This is also the basis for the claim comment you write in Unit 2.

Selection rationale

Fit to interests and time available: #53 is a compact, self-contained bug — a regex fix in pii_scrubber.py — with four named failing tests I can run locally to check my own work before opening a PR. That's a tighter feedback loop than #61 (needs a live stack/DB session to verify) or #49 (an open-ended, 4-7 hour docs task). Given the time I have this week, a bounded fix I can verify myself fits better than either of those.
What the verdict got right vs. what I weighed myself: The verdict correctly confirmed the repo is active, the issue is unclaimed, and the fix is scoped to one file with a concrete bug and reproducible failing tests — exactly what my rubric is built to check. What it couldn't judge is personal fit: whether I actually want to work with regex/parsing logic, and how confident I feel reasoning about phone-number formatting edge cases without deep familiarity with this codebase yet. My rubric's fit-profile field is meant to weigh that once I fill it in — for now I made that call manually.
Anticipated difficulty claiming it: Low. There's no assignee and no open PR against it. Two classmates have pushed fork commits referencing the issue, but Path Review's house rule means that doesn't block me from claiming it — credit attaches to my own PR, not to exclusivity. The main risk is a classmate's PR merging first, which the house rule treats as a non-issue either way.