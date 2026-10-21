# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-faithful | the diff's changed files and changed behavior, read against the plan's in-scope and not-in-scope statements plus any `## Deviations` in plan.md; also the description's fidelity claims read against the diff | every file and behavior change in the diff falls inside what the plan describes (including recorded deviations); the description does not claim more or less than the diff delivers; a shortfall that is honestly recorded in `## Deviations` and described in the PR passes — an unrecorded shortfall or an undisclosed extra does not | required |
| test-evidence | the PR's test-evidence section read against the plan's test plan, and the repo's own check results read against the repo-facts block's stated asks | the evidence shows an observable before and after tied to the issue's reported behavior (the before artifact matches what the repro showed; the after artifact shows the fix working); the repo's own checks were run and their results shown; "tests pass" with no named test or output fails; evidence that re-runs only a path the fix does not touch fails | required |
| diff-clean | the unified diff and the commit list | the diff contains no debug prints, commented-out code blocks, dead functions, re-indentation churn, or edits to files the plan does not name; commits are not labeled wip, fix, or fmt (words that hide what a commit actually changes); a one-line debug leftover counts as debris | required |
| standards-met | the PR description and the repo-facts block's PR template sections and stated contribution policy (including AI-use disclosure) | every section the repo's PR template requires is filled with real content (a placeholder or empty section is a visible ignore); if the repo's stated policy requires disclosing AI usage, the description includes that disclosure; silence in a disclosure-required repo fails | required |

## Verdict rule

Accept if and only if all four required checks pass. Unclear on any required check counts as fail (reject). There is no third verdict.
