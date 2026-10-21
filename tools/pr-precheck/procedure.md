# Procedure: how this tool grades a PR package

## Read order

1. Read the **repo-facts block** first. Note the repo's PR template sections (every section is a claim to fill), the contribution policy, and any AI-use disclosure requirement. These anchor the standards-met check.

2. Read the **plan context** (the plan's diagnosis, scope in-scope/not-in-scope statement, files named, and test plan; and any `## Deviations` block). Record: (a) the full list of files the plan says will change, (b) the test plan's stated before and after observables, (c) any deviation notes that re-tie changes the plan did not originally name. Read the plan BEFORE the diff, so the plan's boundary is established before you see what the diff actually contains.

3. Read the **diff** (the unified diff of changed files and the commit list). For each file and each changed behavior, note whether it falls inside the plan's scope or a deviation note. Note any debug prints, commented-out blocks, dead code, or churn.

4. Read the **description and title**. Note what it claims about the diff's scope, the evidence, and the plan's test plan. Read the description AFTER the diff, so you can check its claims against what you have already seen.

5. Read the **test evidence** section. Note the before artifact, the after artifact, and the repo-checks output. Compare the before artifact to what the repro showed and the after artifact to the plan's test plan.

6. In live mode, fetch the issue thread and the repo's PR template and `docs/CONTRIBUTING.md` to confirm template sections and AI disclosure requirements.

## Evidence gathering

- **plan-faithful**: From the plan, record the in-scope boundary (files named, behavior changes described) and any deviation notes. From the diff, list every changed file and every behavior change introduced. Pair them: mark each diff item as "in plan," "in deviation," or "not in plan or deviation." From the description, extract any fidelity claims ("implements the plan exactly," "no changes outside the plan," "docs now document..."). Compare those claims to the diff.

- **test-evidence**: From the plan's test plan, extract the stated before (what the repro showed) and after (what the fix should produce). From the test-evidence section, extract the before artifact shown, the after artifact shown, and the repo-checks output (make test-unit, lint, typecheck, etc.). Note whether the before artifact matches the repro and whether the after artifact is tied to the fix's behavior (not just a passing path that the fix does not touch).

- **diff-clean**: From the diff, scan every hunk for: debug prints (`eprintln!`, `print(`, `console.log`, etc.), commented-out code blocks (blocks of `//` or `#` that contain real logic), dead functions marked `allow(dead_code)` or similar, re-indentation or whitespace-only hunks, and files not named in the plan or its deviations. From the commit list, note commit messages that are `wip`, `fix`, `fmt`, `cleanup`, or similar one-word labels that describe no specific change.

- **standards-met**: From the repo-facts block, list every section the PR template requires. From the description, check that each section has real content (not a placeholder, not left blank). Note the AI-disclosure requirement; check the description for disclosure text.

## Check execution

Execute the checks in this order:

1. **plan-faithful**: Compare the paired diff items to the plan boundary. If any diff item is neither in the plan's scope nor covered by a deviation note — or if any claimed deviation is not in `## Deviations` in plan.md — grade fail, quoting the undisclosed change or the false fidelity claim. If the description claims the plan is fully implemented but the diff is missing something the plan names, grade fail, quoting the missing item and the claim. If all diff items are in-plan or in-deviation, and the description's claims match the diff, grade pass.

2. **test-evidence**: Check the before artifact: does it show the issue's reported behavior (the crash, the wrong output, the test failing)? Check the after artifact: does it show the specific behavior the plan's test plan names? If either is absent, or if the evidence runs only a path the fix does not affect, grade fail, quoting the missing artifact or the mismatched path. Check the repo-checks output: if the repo's template requires them and they are not shown, grade fail. If before and after are shown and tied to the fix, and required checks are shown, grade pass.

3. **diff-clean**: Scan the diff for debris. If any hunk contains a debug print, commented-out logic, dead code, or unrelated edits to files the plan does not name, grade fail, quoting the debris item. If the commit list contains `wip`, `fix`, `fmt`, `cleanup`, or other opaque one-word labels, note it in the evidence but do not fail on commit style alone unless the rubric adds that; the debris in the diff is the primary signal. If the diff is free of debris and contains only files the plan names (or deviation-covered files), grade pass.

4. **standards-met**: For each section the repo's PR template requires, check that the description contains real content. An empty section or a literal placeholder fails, quoting the missing section. Check the AI-disclosure requirement: if the repo's policy requires disclosure and the description contains none, grade fail. If all required sections are filled and any required disclosure is present, grade pass.

When evidence for a check is genuinely absent (the relevant section of the package is blank), grade unclear. Do not invent evidence.

## Verdict assembly

1. Collect the four grades (pass, fail, or unclear).
2. Apply the verdict rule: accept if and only if all four required checks pass. Any fail or unclear yields reject.
3. In the output JSON, for the deciding check (the first non-pass grade in execution order — plan-faithful, then test-evidence, then diff-clean, then standards-met), quote the one-line evidence that decided it.
4. If all four pass, the verdict is accept; quote the evidence for the test-evidence check in the output (the tightest positive signal).
