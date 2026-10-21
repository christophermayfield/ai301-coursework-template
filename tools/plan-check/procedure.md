# Procedure: how this skill grades a plan package

## Read order

1. Read the **repo-facts block** first. Note the repo's stated contribution policy, especially any AI-use disclosure requirement, and any bug-report template asks. These facts anchor the thread-aware check.

2. Read the **issue context** (title, body, thread highlights). Note: (a) what behavior the issue reports, (b) any maintainer comments that name a culprit, a file, an approach, or request testing. These are the maintainer signals the thread-aware check grades against.

3. Read the **repro evidence block** carefully. For each step, artifact, and control run, note what behavior the evidence pins down and what causes it rules out. Record these as the "repro facts." The diagnosis-grounded check grades the plan's cause against these facts, so they must be noted before the plan is read.

4. Read the **candidate plan** (diagnosis, scope, approach, files, test plan). Read it as a stranger who has the issue and repro evidence in hand, asking: could I start executing this? does the stated cause fit what the repro shows?

5. Read the **candidate plan comment**. Read it against the maintainer signals from step 2 and the contribution policy from step 1.

## Evidence gathering

For each check, gather the following evidence before grading:

- **diagnosis-grounded**: from the repro evidence block, pull the key artifact or control run that pins down the true cause (e.g., a control run showing the bug appears without the suspected component). From the plan, pull the sentence(s) that state the cause. Record both.

- **scope-bounded**: from the candidate plan, list every change described (files to touch, new options, refactors, migrations). Mark each as "addresses the diagnosed cause" or "additional unrequested work."

- **plan-executable**: from the candidate plan's approach section, extract the named starting points: specific files, function names, or code areas. If none are named, record "no file or area named."

- **test-decisive**: from the candidate plan's test plan, extract the stated observable outcome (the specific behavior or artifact that changes after the fix). From the repro evidence, note the behavior the repro showed. Record whether the test plan's observable outcome is tied to the repro behavior.

- **thread-aware**: from the thread highlights (step 2), list every explicit maintainer action (named culprit, pointed-at file, requested testing, posted patch, settled approach). From the plan comment, note whether each action is engaged or ignored. From the repo-facts block, note the AI-use disclosure requirement (if any); from the plan comment, note whether disclosure is present.

## Check execution

Execute the checks in this order. For each check, state the grade (pass, fail, or unclear), then quote the one piece of evidence that decided it.

1. **diagnosis-grounded**: compare the plan's stated cause against the repro facts recorded in the read order. If a control run or artifact in the repro evidence rules the plan's cause out — for example, the control shows the bug appears even without the component the plan blames — grade fail. If the repro evidence is consistent with the diagnosis, grade pass. If the repro evidence block is empty or the plan has no diagnosis at all, grade unclear.

2. **scope-bounded**: check whether every change in the list from evidence gathering is necessary to fix the diagnosed cause. If any change adds refactors, migrations, new options, or structural rewrites the issue never requested, grade fail, quoting the unrequested addition. If all changes address the cause (or explicitly defer extras with a stated reason), grade pass.

3. **plan-executable**: check whether at least one specific file or code area is named in the approach. If nothing is named, or if the entire approach is "investigate" or "profile and optimize" with no named starting point, grade fail. If a file or area is named and the steps are concrete enough for a stranger to start, grade pass.

4. **test-decisive**: check whether the test plan names a specific observable outcome tied to the repro steps. Accepted observables: a named test changing from xfail to pass, a command that produced an error now exiting 0, a specific measurement improving to a stated threshold. Not accepted: "should feel better," "run the test suite," "nothing else should break" without a named outcome. Grade pass if at least one named observable is present; fail otherwise.

5. **thread-aware**: for each explicit maintainer action from evidence gathering, check whether the plan comment engages it. If a maintainer named a culprit file or posted a patch and the plan comment's approach contradicts or ignores it without explanation, grade fail. If no maintainer signals exist, this check grades pass (nothing to engage). Then check AI disclosure: if the repo requires it and the comment does not include it, grade fail regardless of the thread signals.

When evidence for a check is genuinely absent (the relevant section of the package is blank or the package has no repro evidence at all), grade unclear. Do not invent evidence.

## Verdict assembly

1. Collect the five grades (pass, fail, or unclear).
2. Apply the verdict rule: accept if and only if all five required checks pass. Any fail or unclear yields reject.
3. In the output JSON, for the deciding check (the first non-pass grade in execution order), quote the one-line evidence that decided it.
4. If all five pass, the verdict is accept; quote the evidence for the test-decisive check in the output (the last check, and often the tightest).
