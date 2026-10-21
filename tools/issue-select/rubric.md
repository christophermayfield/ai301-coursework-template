# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-alive | "archived:" and "last push to any branch" in the repo facts block | archived is "no" AND the last push to any branch is within 365 days of the bundle's capture date | required |
| maintainer-alive | "maintainer first-response sample" (5 entries) and "last 5 default-branch commits" authors in repo facts | at least 2 of the 5 sampled issues show a maintainer first-response time (i.e., the entry is not "no maintainer comment"), OR at least 3 of the last 5 default-branch commits are authored by a human (username does not end in [bot]) | required |
| scope-fits | issue body and the full comment thread | the issue describes one bounded change: it is not a tracking or umbrella issue (a list of sub-tasks or work items meant to be split into separate contributions); it is not a pure usage or support question; and the implementation approach and acceptance criteria are settled (a maintainer has stated what needs to be done, or the issue body is specific enough to proceed without an unresolved product or design decision from a maintainer). A long thread, an open-for-years date, or a terse issue body does not fail this check by itself. When a maintainer opened the issue or added a good-first-issue label, treat the scope as settled even if the body is terse: maintainer judgment counts as the spec. An issue naming multiple implementation approaches or listing additional suggestions for a single underlying bug or feature is still one bounded change, not a tracking issue; what matters is that only one root problem or goal is being addressed. Two or more closed linked PRs that did not merge fail this check, as they are evidence of hidden implementation difficulty. | required |
| unclaimed | "assignees:" and "linked PRs:" in the repo facts block, and claim comments in the comment thread | no assignee is set AND there are no open linked PRs; stale claim comments (no maintainer endorsement of the claim AND the comment is 90 or more days before the capture date with no follow-up activity) do not block | required |
| ai-policy | "contribution policy" line in the repo facts block | the policy does not contain an outright ban on AI-generated contributions; disclosure requirements, personal-understanding requirements, and human-review requirements all pass; silence passes | required |
| maintainer-engaged | author_association tags in the comment thread and the issue opener's association | at least one comment is from an OWNER, MEMBER, or COLLABORATOR, or the issue was opened by a maintainer | preferred |

## Verdict rule

Accept if and only if all five required checks pass. Preferred checks never change the verdict; they are used only to rank accepted issues against the fit profile in scope.md. Unclear on any required check counts as fail (reject). There is no third verdict.
