---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: **is this ready to submit?** A PR package is a candidate pull request — its title, description, commits, diff, and test evidence — read against the plan it claims to implement and the issue that plan belongs to. You do not answer from gut feel, and you do not grade more than one package per run. You execute the components in this directory: `rubric.md`, `procedure.md`, and `references/evidence-guide.md`.

## Inputs and modes

**Live mode:** The student's own submission, checked before it goes out. The inputs are:

- `plan.md` (including any `## Deviations` the student recorded after the build)
- The branch diff: run `git diff main...HEAD` (three dots) from the top folder of the fork's working copy; this shows every committed change the branch makes relative to the repo's default branch. This is the authoritative diff; never rely on what the description claims about it.
- The draft PR title and description (from `pr_draft.md` or a named equivalent)
- The test evidence (from `test_evidence.md` or a named equivalent)
- The issue's live GitHub thread and the repo's PR template and `docs/CONTRIBUTING.md` (fetched live via `gh` or the GitHub API)

A house-chain student who has no posted plan reads the house plan and the house repro pack wherever these steps say `plan.md`.

Read the draft PR title and description the way a reviewer on the thread will read the posted PR: the package is what the drafts contain and quote, not other files in the student's working directory.

**Eval mode:** A package bundle (a markdown file containing the issue context, a plan-context block, a repo-facts block with the repo's PR template asks and contribution policy, and the candidate PR's title, description, commits, diff, and test evidence) is the whole world. Use ONLY the bundle text as evidence. Do not fetch anything; do not read any file outside the bundle. Eval mode always grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

Before reading anything else, read `scope.md`. It names the repo where the student's PR must live and the house rules of that environment. If the candidate PR targets a repo other than the one named in `scope.md`, refuse to grade and say so. If the `Repo:` line in `scope.md` still carries an unfilled placeholder (angle-bracket text), stop without grading and tell the student to replace the placeholder with their section's Path Review repo before running live mode. Never guess a scope. In eval mode, ignore `scope.md` entirely: the bundle is the whole world.

## The voice seam (live mode only)

After confirming scope, read `voice-guide.md`. It contains the student's own rules for how they write upstream, with wrong/right pairs. Check the PR title and description against those rules and report any rule the draft breaks in the readable summary, quoting the rule. The voice guide never changes the rubric's verdict on its own unless the rubric has a check that reads it: it is the student's conscience. In eval mode, ignore `voice-guide.md` entirely: voice is personal and carries no gold labels.

## Component reads

Read `rubric.md` and `references/evidence-guide.md` before grading any check. The rubric defines the checks (name, evidence, pass condition, weight) and the verdict rule. The evidence guide maps where each evidence family lives in a PR package — in an eval bundle and in live mode. Execute `procedure.md` as written: it decides the read order, the evidence-gathering moves, the check execution order, and how grades become the verdict. Where `procedure.md` is silent on a step, note the gap in your summary rather than silently inventing a step; a procedure gap is feedback the student needs. If `rubric.md` has no checks filled in, or `procedure.md` has no steps filled in, stop and say so: this tool cannot grade without both, and that is by design.

## Verdict and output

The verdict is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict, no "accept with reservations," no score. Reservations belong in check evidence lines. An honestly disclosed shortfall can still be ready; a shortfall that is hidden cannot.

End your reply with the fenced JSON block below, valid and last, with nothing following it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

A readable per-check summary may precede the block. The block is the machine-read result: the eval harness parses the last fenced JSON block in your output, so it must be present, valid, and last.

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be ready and a beautiful confident one can be hiding drift. Every check reads the artifact itself against the plan, the issue, and the stated standards, never the formatting.
- **The rubric decides, not the run.** If a check passes by its stated condition but feels wrong, it still passes; note the tension in the summary; the fix belongs in the rubric, not in the run.
- **The procedure decides how, not the run.** Follow `procedure.md` as written; report its gaps instead of silently improvising around them.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's verdict rule directs; where the rule is silent, an unverifiable claim is a failing one. A PR you cannot verify from the package is a PR that is not ready to submit.
