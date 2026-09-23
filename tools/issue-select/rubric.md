# Rubric: is this a good first issue?

<!--      
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:
  
1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
|Repo is active  | repo-facts block: date of the most recent commit on the default branch  | Most recent default-branch commit is within the last 30 days |required  |
| Maintainer is alive | 	repo-facts block or comment thread: dates of maintainer comments across issues/PRs  | At least one maintainer comment on any issue or PR within the last 30 days | required |
|Not already claimed | 	Issue metadata: assignee field; comment thread: claim language ("I'll take this", "working on this") or a linked PR  | Issue has no assignee, AND no comment claims it or links a PR to it | required |
| Scope fits a newcomer | 	Issue body: labels present; description content  |Issue is labeled good-first-issue/beginner-friendly, OR the body names a specific file, function, or reproducible error — not an open-ended feature request or architecture question  | required |
|  |   |  |  |


## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Unclear on any check counts as a fail for that check.

Reject if any required check fails (including unclear). Accept if all four required checks pass.

Preferred checks never change accept/reject. Among accepted issues, rank by number of preferred checks passed — more passing preferred checks means a better fit.
