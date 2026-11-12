# Contribution: Add support for OPTICAL_FLOW_RAD

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Christopher Mayfield
**GitHub Username:** christophermayfield
**Issue:** https://github.com/ArduPilot/ardupilot/issues/31588
**Pull Request:** [not yet opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Installed issue-scout, wrote a three-query Python search strategy, ran a full wild scan, and graded candidate issues including my chosen one**

I installed the issue-scout skill from the unit 5 starter, carried my fit profile forward, and wrote a search strategy with three `gh search issues` queries targeting Python good-first-issue bugs and documentation issues. The full scan returned 220 survivors from 260 unique results after dropping assigned and stale issues; all five graded candidates came back accept. I then graded two hand-picked issues from the Issue Selection site: refined-saved-replies#2 rejected on unclaimed (open linked PR) and ardupilot#31588 also rejected on unclaimed (open linked PR #31601) and outside my Python fit profile. I chose ardupilot#31588 anyway — it is a real actively-maintained project with an engaged maintainer and a clearly scoped feature request. The rubric's unclaimed check correctly caught both open PRs; nothing needed patching there.

**Unit 6: Installed voice guard with Upstream section, wrote CLAUDE.local.md for the ArduPilot clone, and posted a claim on issue #31588**

The voice guard was already wired from earlier unit work; the new piece was the Upstream section in the voice guide (rules for claims, questions, and pings to maintainer strangers) and a CLAUDE.local.md for the ArduPilot repo (conventions from CONTRIBUTING.md, the 4–5 files the issue touches, build commands, the design question peterbarker's thread raised). The repro-check graded both the claim and the source-inspection repro as accept; the claim was posted through Claude Code so the voice guard ran first and confirmed no rule was broken. One thing that fell short: scope-feasible was already patched in unit 5, so there was nothing to patch this week — the difficulty note correctly flagged the issue as ambitious and that flag became the first question in my claim.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My issue-scout ran three `gh search issues` queries targeting Python good-first-issue bugs, documentation issues, and general good-first-issues. The full scan returned 220 survivors from 260 unique results after dropping assigned and stale issues; all five graded candidates came back accept. I then graded two issues from the Issue Selection site in direct mode.

The first pick, refined-saved-replies#2, was rejected on unclaimed (open linked PR). The second, ardupilot#31588 ("Add support for OPTICAL_FLOW_RAD"), was also rejected on unclaimed (PR #31601 by redn1nja, opened July 2026) and flagged outside my Python fit profile. The scout's difficulty note said ambitious: an open design edge (tpwrules raised whether OPTICAL_FLOW_RAD is truly parallel to OPTICAL_FLOW or requires different handling because it carries pre-integrated values and its own gyro samples), and the fix touches C++ embedded firmware, not Python.

I chose it anyway for two reasons. First, the gap is unambiguous: `AP_OpticalFlow_MAV.cpp` handles `MAVLINK_MSG_ID_OPTICAL_FLOW` with no handler at all for `OPTICAL_FLOW_RAD`, confirmed by a code search that finds the message ID only in auto-generated MAVLink headers. Second, the maintainer peterbarker gave specific direction in the thread (`AP_MAVLINK_MSG_OPTICAL_FLOW_RAD_ENABLED` macro, entries in `build_options.py`), so the first question is design rather than discovery. I want to learn how to read an embedded C++ codebase, navigate a build-flag system, and work through a design question with an upstream maintainer rather than waiting for a perfect Python issue.

The scout's unclaimed check was correct — PR #31601 is open — and I accepted the risk. The PR has been inactive for 4 months, and ArduPilot explicitly says it does not assign issues. My plan is to check whether PR #31601 is still being actively developed before writing any code, and join that PR's discussion if it is.

Claim comment: https://github.com/ArduPilot/ardupilot/issues/31588#issuecomment-6810677138

**Toolkit**

- Tool: issue-scout (direct-mode grade of ardupilot#31588; full scan for Python issues; scope-feasible patch from unit 5 already in place)
- What it produced: reject verdict on unclaimed + ambitious difficulty note naming the design edge on message format differences
- Difficulties: the scout rejected the issue I wanted; unclaimed correctly caught the open PR; fit profile correctly flagged C++ as outside my stated comfort zone
- What changed: chose the issue despite the reject; the story explains why; no rubric change needed since the checks were right

---

## Understanding the Issue

**Problem Description**

[To be filled in.]

**Expected Behavior**

[To be filled in.]

**Current Behavior**

[To be filled in.]

**Affected Components**

[To be filled in.]

**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would update, or "nothing"]

---

## Reproduction Process

**Environment Setup**

ArduPilot repository at commit `546cf152a8c868acbac82dd326f9649da66d6ba1` (master, 2026-11-11), inspected via GitHub API and raw file fetch. This is a C++ embedded firmware project requiring an arm-none-eabi cross-compiler for a full build; the gap is demonstrated by static source inspection, which does not require a local build environment.

**Steps to Reproduce**

1. Open `libraries/AP_OpticalFlow/AP_OpticalFlow_MAV.cpp`. The `handle_msg()` method handles `MAVLINK_MSG_ID_OPTICAL_FLOW`: it decodes the packet, accumulates `flow_x/y` or `flow_rate_x/y` fields, and stores quality and sensor ID. No case or handler for `MAVLINK_MSG_ID_OPTICAL_FLOW_RAD` exists in this file.

2. Search the file and the broader source tree for `OPTICAL_FLOW_RAD`. The message ID and its decode function (`mavlink_msg_optical_flow_rad_decode()`) appear only in auto-generated MAVLink headers under `mavlink/v2.0/common/` — nowhere in ArduPilot's own driver or GCS code.

3. Check `libraries/GCS_MAVLink/GCS_Common.cpp`. The routing table contains `{ MAVLINK_MSG_ID_OPTICAL_FLOW, MSG_OPTICAL_FLOW }` with no corresponding entry for `MAVLINK_MSG_ID_OPTICAL_FLOW_RAD`.

**Reproduction Evidence**

OPTICAL_FLOW handler present in `AP_OpticalFlow_MAV.cpp`:
https://github.com/ArduPilot/ardupilot/blob/546cf152a8c868acbac82dd326f9649da66d6ba1/libraries/AP_OpticalFlow/AP_OpticalFlow_MAV.cpp

OPTICAL_FLOW_RAD absent — GitHub code search `OPTICAL_FLOW_RAD repo:ArduPilot/ardupilot` returns zero matches in C++ source files (only auto-generated MAVLink headers). A companion computer sending `OPTICAL_FLOW_RAD` gets no response from the flight controller.

No commit link — repository not cloned locally; inspection was done via GitHub API and raw file access.

**Toolkit**

- Tool: repro-check (graded claim-draft.md + repro-draft.md together against issue #31588)
- What it produced: accept verdict; all five checks passed including comms-ok (CONTRIBUTING.md has no AI policy; claim references stale PR and raises design question)
- Difficulties: "reproduction" for a missing feature means showing the absence rather than a crash; the evidence guide's behavior-matches check passes on a confirmed gap, but the method (source inspection without a local build) is unusual and I stated it explicitly to pass honest-outcome
- What changed: nothing; the package was posted as-is after the voice guard confirmed no rule was broken

---

## Solution Approach

**Analysis**

[To be filled in.]

**Proposed Solution**

[To be filled in.]

**Implementation Plan**

[To be filled in.]

**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would update, or "nothing"]

---

## Testing Strategy

[To be filled in.]

**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would update, or "nothing"]

---

## Implementation Notes

[To be filled in as work progresses.]

**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would update, or "nothing"]

---

## Pull Request

**PR Link:** [not yet opened]

**PR Description:** [To be filled in.]

**Maintainer Feedback:**

[To be filled in.]

**Status:** Not yet opened

**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would update, or "nothing"]

---

## Learnings and Reflections

**Technical Skills Gained**

[To be filled in.]

**Challenges Overcome**

[To be filled in.]

**What I'd Do Differently Next Time**

[To be filled in.]

---

## Resources Used

- ArduPilot issue #31588: https://github.com/ArduPilot/ardupilot/issues/31588
