# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57

**Verdict output**

```
Grading Results

Repo-level facts (same for all three — same repo)
- archived: no ✓
- Last push: 2026-09-16 (35 days before today — well within 365) ✓
- Last 5 default-branch commits: all by Aburke225 (human, no [bot] suffix) → 5/5 human ✓
- AI policy: docs/CONTRIBUTING.md has no statement on AI tooling — silence passes ✓

Path Review house rule note: The house rule states "ignore the usual claim signals here" and "course credit attaches to the pull request you open, not to whether it merges, so a shared issue costs nobody anything." All linked PRs on these issues are from NONE-association users (students). I treat student-authored open PRs as non-blocking under the house rule's broad directive to ignore student claim signals; formal assignees set by repo staff would still block.

Issue #57 — Tech detector counts vendored and build-output files

| Check | Grade | Evidence |
|---|---|---|
| repo-alive | pass | archived: no; last push 2026-09-16, 35 days ago |
| maintainer-alive | pass | 5/5 default-branch commits by Aburke225 (human) |
| scope-fits | pass | Single bounded bug with inline reproduction script, two named failing tests, COLLABORATOR-opened with good-first-issue label |
| unclaimed | pass | No assignee; 1 linked PR (#82) by whitgunt77 (NONE association — student, non-blocking under Path Review house rule) |
| ai-policy | pass | docs/CONTRIBUTING.md: no statement on AI tooling — silence passes |
| maintainer-engaged | pass (preferred) | Issue opened by Aburke225 (COLLABORATOR) |

Verdict: ACCEPT

Ranked accepted candidates:
1. #57 — Tech detector (top fit): Clearest self-contained reproduction script in the issue body, Python agent code squarely in the fit profile, only one competing student PR.
2. #72 — verify_password: Simplest possible fix but has 23 open student PRs.
3. #69 — Output parser: Also a clean Python fix, but more student activity than #57.
```

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "isArchived: false; last push 2026-09-16, 35 days before today"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "5/5 default-branch commits by Aburke225 (human, no [bot] suffix)"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Single bounded bug with inline reproduction script, two named failing tests, COLLABORATOR-opened with good-first-issue label"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignee; 1 linked PR (#82) by whitgunt77 (NONE association — student, non-blocking under Path Review house rule)"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md: no statement on AI tooling — silence passes"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Opened by Aburke225 (COLLABORATOR)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "isArchived: false; last push 2026-09-16, 35 days before today"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "5/5 default-branch commits by Aburke225 (human, no [bot] suffix)"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Single bounded bug: catch UnknownHashError, return False; specific files and xfail test named; COLLABORATOR-opened good-first-issue, 1–2 hour estimate"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignee; 23 linked PRs all by NONE-association students — non-blocking under Path Review house rule"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md: no statement on AI tooling — silence passes"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Opened by Aburke225 (COLLABORATOR)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "isArchived: false; last push 2026-09-16, 35 days before today"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "5/5 default-branch commits by Aburke225 (human, no [bot] suffix)"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Single bounded bug: output_parser.py calls .items() on list; specific files and xfail test named; COLLABORATOR-opened good-first-issue, 2–4 hour estimate"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignee; 6 linked PRs all by NONE-association students — non-blocking under Path Review house rule"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md: no statement on AI tooling — silence passes"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Opened by Aburke225 (COLLABORATOR)"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Run 1 (partial, 2 issues): agreement 2/2 — smoke test confirming the harness worked under `env -u CLAUDECODE`.

Run 2 (partial, 6 issues — issue-07, issue-09, issue-12, issue-15, issue-18, issue-20): agreement 5/6. Miss: issue-15 graded accept (gold: reject). The rubric had no signal for two abandoned closed PRs.

Run 3 (partial, 2 issues — issue-09, issue-15 after adding the closed-PR rule): agreement 2/2. Fix confirmed.

Run 4 (full, 20 issues): agreement 18/20 (bar: 18/20: PASS). Misses: issue-04 and issue-19 both graded reject on scope-fits despite gold accept. Both are maintainer-filed, terse-body issues from zxcalc/zxlive.

Run 5 (partial, 6 issues — issue-04, issue-05, issue-10, issue-15, issue-19, issue-20 after adding maintainer-filed and multi-approach language to scope-fits): agreement 6/6.

Run 6 (full, 20 issues — confirming run): agreement 20/20 (bar: 18/20: PASS).

**Issue analysis**

issue-15 (source: zulip/zulip#19589, category: scope, gold: reject).

My rubric decided: **accept**. Gold label: **reject**. The gold note reads "years of design debate and two abandoned PRs behind a friendly label."

My rubric's scope-fits check read the issue as passing because the body describes a single bounded change (separate the `command` field from `text` in outgoing webhook payloads), maintainers engaged in the thread (timabbott, eeshangarg), and the Slack API documentation was eventually found. The check's original wording said "A long thread, an open-for-years date, or a terse issue body does not fail this check by itself," which the model interpreted charitably: the design looked settled enough to proceed.

What the rubric missed was that the two linked closed PRs (zulip/zulip#20840 and zulip/zulip#23123) are a concrete signal that multiple people tried and abandoned the implementation. The evidence guide says "an issue open for years with several abandoned attempts (closed, unmerged PRs in its history) is telling you something about its real difficulty," but my rubric had not encoded this as a fail condition. After adding "Two or more closed linked PRs that did not merge fail this check, as they are evidence of hidden implementation difficulty," the rubric correctly rejected issue-15.

**Check rationale**

Current wording (from the uploaded rubric.md):

> "Two or more closed linked PRs that did not merge fail this check, as they are evidence of hidden implementation difficulty."

This clause was added after Run 2 revealed that issue-15 was being graded accept despite two closed abandoned PRs. The evidence guide explicitly calls out "abandoned attempts (closed, unmerged PRs in its history)" as a scope signal, but the original scope-fits pass condition only listed three explicitly non-failing items (long thread, open-for-years date, terse body). Without an explicit fail condition for closed PRs, the model did the natural thing: it read the issue charitably since those three non-failures were the only named scope signals. The new clause gives the model a concrete fail signal tied to a countable, unambiguous fact in the repo-facts block ("linked PRs: … (closed)"), which is exactly the kind of evidence a rubric check should anchor to.

**Trade-offs**

The "two or more closed PRs" threshold was chosen to keep issue-09 (conda/conda#7617) passing — it has one closed linked PR (conda/conda#11627) and the gold label is accept. A threshold of one closed PR would incorrectly reject issue-09. A threshold of two preserves that accept while still catching issue-15 with its two closed PRs.

To verify nothing else broke, I re-ran the full 20-issue eval after adding the clause (Run 6) and confirmed 20/20. The scope-category issues that were already correct (issue-05, issue-10, issue-20) remained reject, and no previously-accepted issue flipped to reject. I also re-ran issue-09 individually in Run 3 (before the maintainer-filed additions) to confirm it still passed unclaimed and scope-fits with one closed PR.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to interests and time available.** The tech-detector bug is a pure Python fix in agent/tools/tech_detector.py: exclude node_modules/ and build/ directory paths from the file-language count. I have hands-on Python experience and have done exactly this kind of path-filtering work before. The issue body includes a four-line reproduction script and names two specific failing tests, so I can confirm the bug in minutes. The scope is narrow enough to finish in a focused afternoon session.

2. **What the verdict identified correctly, and what I weighed that the rubric could not.** The rubric correctly flagged that this is a bounded, maintainer-opened, unclaimed (no formal assignee) issue in an active repo with a permissive AI policy. What the rubric cannot see is that the issue has the fewest competing student PRs of the three accepted candidates (one, compared to six and twenty-three), which in a classroom context means I am less likely to be working against a nearly-merged submission when I open my own PR.

3. **Anticipated difficulty in claiming it.** The claim itself is straightforward — the issue is in the scoped repo and the house rule says classmates' PRs don't block. The technical work is simple path filtering with no external dependencies. The main risk is that one other student (PR #82) may already be close to done; but since course credit attaches to the PR I open, not to whether it merges, this is not a reason to pass on the issue.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
