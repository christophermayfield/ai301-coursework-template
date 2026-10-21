# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | the plan's stated cause read against the repro evidence block (the steps, artifacts, and control runs that pin down the behavior) | the diagnosis follows from what the repro evidence actually shows: it does not contradict a control run or ignore an artifact that rules the stated cause out; a plan that picks up the thread's own confirmed diagnosis passes if the repro evidence is consistent with it | required |
| scope-bounded | the plan's in-scope and not-in-scope statements, and the full set of changes and files described | the plan makes one bounded change that addresses the diagnosed cause; it does not add unrequested refactors, migrations, new options, UI rework, or structural rewrites the issue never asked for; a plan that explicitly defers additional work and says why passes | required |
| plan-executable | the plan's approach and the files or areas it names | a stranger could start executing this plan without asking the author anything: the approach names concrete steps and at least one specific file or code area to touch; "investigate and optimize" or "poke around" with no named starting point fails | required |
| test-decisive | the plan's test plan read against the repro evidence's steps and artifacts | the test plan names at least one observable outcome tied to the repro steps: a command that was failing now exits 0, a test that was xfail now passes, a specific artifact changes in a stated direction; "should feel better", "run the full test suite", or "nothing else should break" with no named observable outcome fails | required |
| thread-aware | the plan comment read against the thread highlights and the repo-facts block's contribution policy (including any AI-use disclosure requirement) | the plan comment does not ignore explicit maintainer direction present in the thread (a named culprit, a pointed-at file, an approach posted, testing requested); if the repo's stated policy requires disclosing AI usage, the comment discloses | required |

## Verdict rule

Accept if and only if all five required checks pass. Unclear on any required check counts as fail (reject). There is no third verdict.
