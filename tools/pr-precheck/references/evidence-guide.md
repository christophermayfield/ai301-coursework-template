# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives:**
- In an eval bundle: the plan-context block's in-scope/not-in-scope statements and files list; any `## Deviations` section in the plan text; the candidate PR's unified diff (changed files and changed behaviors); the description's fidelity claims ("implements the plan exactly," "no changes outside the fix").
- In live mode: `plan.md`'s scope section and `## Deviations`; the diff from `git diff main...HEAD`; `pr_draft.md`'s description.

**What good looks like:**
Every file changed in the diff is named in the plan's scope or a deviation note. Every behavioral change in the diff is described in the plan or a deviation note. The description's fidelity claims match what the diff shows: if the description says "implements the plan exactly," the diff must not contain files or changes the plan never mentioned. Silent drift in the more-than-plan direction: the diff touches files the plan scopes out, with no deviation note. Silent drift in the less-than-plan direction: the diff skips something the plan names, and the description claims completeness without noting the gap. An honest deviation recorded in `## Deviations` re-ties the mismatch: "I added X instead of Y because Z" passes; a diff that quietly adds X with no explanation does not.

## Test evidence (harness category: not-tested)

**Where it lives:**
- In an eval bundle: the candidate PR's test-evidence section, containing before artifacts, after artifacts, and repo-check outputs; the plan-context block's test plan (what the before should show and what the after should show).
- In live mode: `test_evidence.md`; the plan's test plan in `plan.md`; the repo's stated checks from the PR template and `docs/CONTRIBUTING.md`.

**What good looks like:**
The before artifact shows the issue's reported behavior (the crash, the wrong output, the failing test) — the same behavior the repro showed. The after artifact shows the specific behavior the plan's test plan names as the success criterion. The repo's own checks (make test-unit, lint, typecheck, etc.) are run and their output shown. Not good: "tests pass" with no named test or output; evidence that runs only a path the fix does not affect (the unchanged control path); repo checks that the template requires but are never shown. An honest cannot-reproduce or a failing check reported with a stated reason passes; hiding a failure does not.

## Diff quality (harness category: unreviewable)

**Where it lives:**
- In an eval bundle: the unified diff and the commit list in the candidate PR section.
- In live mode: `git diff main...HEAD` output; `git log main...HEAD --oneline`.

**What good looks like:**
The diff shows exactly the change the plan describes, and nothing else. A reviewable diff: the fix is visible as the primary change; every file in the diff is named in the plan or its deviations; commit messages name what changed. Debris signals: debug prints left in (eprintln!, print(, console.log, dbg!); commented-out code blocks containing real logic; dead functions guarded by `allow(dead_code)` or `#[allow(unused)]`; re-indentation or whitespace-only hunks that bury the fix in noise; edits to files the plan never touched with no deviation note; commit messages labeled `wip`, `fix`, `fmt`, `misc`, or `cleanup` with no further description. A single-line TODO in an import comment or a trailing whitespace fix does not fail by itself; a multi-line commented-out first attempt does.

## Standards and comms (harness category: standards-wall)

**Where it lives:**
- In an eval bundle: the repo-facts block's PR template sections and stated contribution policy (especially AI-use disclosure requirements); the candidate PR's description against those sections.
- In live mode: the repo's `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` (fetched via `gh api`); `pr_draft.md`'s description.

**What good looks like:**
Every section the PR template requires is filled with real content. Required sections vary by repo; common ones: summary/description, issue reference (`Closes #N`), change list, testing evidence, and any AI-disclosure section. An empty section, a placeholder line, or a section that is visibly copied from the template with no content filled in fails. If the repo's contribution policy requires disclosing AI usage ("all AI usage must be disclosed"), the PR description includes that disclosure: the tool used, the extent of the assistance. Silence in a disclosure-required repo is a standards failure regardless of how good the rest of the PR is.
