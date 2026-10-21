# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | the repro report's environment section, read against the issue's stated target (OS, version, driver/backend if the issue is platform-specific) | the report names the tool version and the OS or platform; if the issue is specific to a driver, backend, or install method, those details are present too; a version that differs from the issue's target passes if the difference is called out explicitly | required |
| steps-followable | the repro report's steps, read against the issue's trigger description | steps describe a starting state a stranger can reach, include every command or action needed to hit the behavior, and do not require access to private resources (private repos, unshared configs, proprietary infra); omitting a step whose absence changes whether the trigger fires is a fail | required |
| behavior-matches | the artifact(s) shown in the repro report (output excerpt, error message, log, screenshot) read against the issue's described behavior | the artifact shows the specific behavior the issue reports — the same error type, the same crash exit code, the same wrong output — not an adjacent outcome (a graceful validation error in place of a crash, garbled output in place of a hang); an honest cannot-reproduce with a shown attempt and a statement of what differed passes; a confident "reproduced" with no artifact fails | required |
| honest-outcome | the claims in the repro report compared to the artifacts shown | every assertion of reproduction is backed by a shown artifact whose content matches the claim; the report does not narrate the issue's described behavior over an artifact that shows something different; uncertainty is stated rather than suppressed; a cannot-reproduce stated as such with evidence of what was tried passes | required |
| comms-ok | the claim comment read against the issue's specifics, and the repo-facts block's contribution policy (including any AI-use disclosure requirement) | the claim comment names this specific issue and describes concrete next steps — no guaranteed fixes, no delivery timelines, no interchangeable boilerplate that could go on any issue in the repo; if the repo's stated policy requires disclosing AI usage, the submitted comments must include that disclosure | required |

## Verdict rule

Accept if and only if all five required checks pass. Unclear on any required check counts as fail (reject). There is no third verdict.
