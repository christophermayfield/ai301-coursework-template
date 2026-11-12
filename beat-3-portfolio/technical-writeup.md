# Adding OPTICAL_FLOW_RAD support to ArduPilot, with AI-assisted tools checking my work

## Overview

This term I contributed a MAVLink message handler to ArduPilot, a 16,000-star open-source
autopilot used in real drones and rover vehicles. The issue (#31588) was straightforward to
diagnose: ArduPilot's optical flow driver handled `OPTICAL_FLOW` but silently dropped
`OPTICAL_FLOW_RAD`, a companion message that delivers pre-integrated flow data with its own
gyro samples. I added the handler across five files in three subsystems, gated it on a
compile-time flag per the maintainer's direction, and opened PR #35145 against `master`. The
PR is currently open and awaiting review; a build failure I introduced (a macro not in scope
in one include context) was caught by CI on the first run and fixed in two additional commits.
I also built a suite of AI-assisted grading tools over units 1–4 of the course, and used them
directly to plan, check, and gate the contribution.

PR: https://github.com/ArduPilot/ardupilot/pull/35145

## What happened

**Finding the issue.** I used my issue-scout tool to search GitHub for first-contribution
candidates. The scout ran three `gh search issues` queries and graded the top five results with
a rubric I wrote in unit 1, then iterated. When I graded ArduPilot issue #31588 directly, the
scout returned a `reject` verdict on the `unclaimed` check — an open linked PR (#31601) from
another contributor. It was right: the PR existed. But it was four months inactive. I chose the
issue anyway and recorded the reason in my story:

> Scout output (from `~/.claude/skills/issue-scout/scan-output.md`):
> ```json
> {"name": "unclaimed", "grade": "fail",
>  "evidence": "PR #31601 is open and cross-referenced; PR #31805 is closed"}
> ```

The scout's earlier rubric would have rejected the issue for a different reason. In unit 1, I
wrote a `scope-fits` check that required the fix approach to be fully settled before I'd take
the issue. When I ran the scout on wild issues, it rejected most of them: real maintainers
rarely spell out the exact approach in advance. I patched the check, documented in
`scan-notes.md`:

> **Before (scope-fits):** required the implementation approach and acceptance criteria to be
> fully settled.
>
> **After (scope-feasible):** fails only when there is no single identifiable piece of work at
> all; open design edges raise the difficulty note instead of rejecting.

**Tracing the gap.** I confirmed the bug by reading the source directly: `AP_OpticalFlow_MAV.cpp`
handles `MAVLINK_MSG_ID_OPTICAL_FLOW` via a `handle_msg()` function, but no case for
`MAVLINK_MSG_ID_OPTICAL_FLOW_RAD` exists in the driver or in the GCS routing switch in
`GCS_Common.cpp`. The code-search result — zero matches for `OPTICAL_FLOW_RAD` in any ArduPilot
driver or GCS file — was the before-state proof.

**Building the fix.** The handler converts OPTICAL_FLOW_RAD's pre-integrated radians to rad/s
using the message's own integration interval, then feeds the existing `flow_sum_is_rads`
accumulation path:

```cpp
// OPTICAL_FLOW_RAD gives pre-integrated flow in radians over
// time_delta_distance_us microseconds; convert to rad/s so the
// existing flow_sum_is_rads accumulation path can be reused
const float dt_inv = 1.0e6f / packet.time_delta_distance_us;
flow_sum_is_rads = true;
flow_sum.x += packet.integrated_x * dt_inv;
flow_sum.y += packet.integrated_y * dt_inv;
```

The change spans five files in three subsystems: `AP_OpticalFlow` (config, backend, frontend,
MAV driver), `GCS_MAVLink` (config macro, GCS header, routing), and `Tools`
(build_options.py).

**The ship-gate rejection.** I ran my pr-precheck tool (built in unit 4) through the ship-gate,
which orchestrates repo checks and then pr-precheck on my branch. The first run rejected the PR:

> Ship-gate log (`~/.claude/ship-gate/gate-log.md`):
> ```
> standards-met [fail]: Single commit 'AP_OpticalFlow: add OPTICAL_FLOW_RAD MAVLink message
> support' changes libraries/GCS_MAVLink/GCS.h, GCS_Common.cpp, GCS_config.h, and
> Tools/scripts/build_options.py — three subsystems under one prefix; AGENTS.md §7 and CI
> enforce one subsystem per commit.
> ```

My pr-precheck's `standards-met` check read ArduPilot's `AGENTS.md` and correctly caught that
ArduPilot CI enforces one subsystem per commit. I reset the commit, split the change into three
subsystem-prefixed commits, and the gate passed on the second run:

> ```
> pre-commit hooks (style, lint, copyright): ok (5s)
> pysim Python unit tests: ok (1s)
> plan-fidelity [pass] / test-decisive [pass] / diff-clean [pass] / standards-met [pass]
> ship-gate: PASS
> ```

**The CI build failure.** After the PR opened, 65 of ArduPilot's CI build jobs failed. The
error was:

```
../../libraries/GCS_MAVLink/GCS.h:751:5: fatal error:
'AP_OPTICALFLOW_ENABLED' is not defined, evaluates to 0 [-Wundef]
   751 | #if AP_MAVLINK_MSG_OPTICAL_FLOW_RAD_ENABLED
../../libraries/GCS_MAVLink/GCS_config.h:128:49: note: expanded from macro
'AP_MAVLINK_MSG_OPTICAL_FLOW_RAD_ENABLED'
   128 | #define AP_MAVLINK_MSG_OPTICAL_FLOW_RAD_ENABLED AP_OPTICALFLOW_ENABLED
```

I had defined `AP_MAVLINK_MSG_OPTICAL_FLOW_RAD_ENABLED` in `GCS_config.h` using
`AP_OPTICALFLOW_ENABLED`, but `AP_OPTICALFLOW_ENABLED` is defined in
`AP_OpticalFlow_config.h` — which is not guaranteed to be included before `GCS_config.h` in
all build contexts. I fixed it in two more subsystem commits: adding the macro to
`AP_OpticalFlow_config.h` itself, and adding an explicit include of `AP_OpticalFlow_config.h`
to `GCS.h`. Both pre-commit and pysim tests still pass after the fix.

**Where it landed.** PR #35145 is open on `ArduPilot/ardupilot master` with CI re-running
after the build fix. The pr-monitor tool I installed in unit 8 read the PR state and drafted a
"wait" note (nudge threshold is 21 days; the PR has been open less than 24 hours). The PR is
awaiting a human review.

## Limitations and what's next

The fix does not use `OPTICAL_FLOW_RAD`'s own integrated gyro samples
(`integrated_xgyro/ygyro/zgyro`). Following the maintainer's direction to treat the message
the same way as `OPTICAL_FLOW` except for the radians conversion, I used AHRS gyro instead —
the same as the existing handler. That choice is flagged explicitly in the PR description for
the maintainer to redirect if needed. If they prefer the message's own gyro data, the handler
would need to bypass the AHRS gyro path in `update()`, which is a larger structural change.

I also could not run a full C++ build locally on macOS without an ARM cross-compiler, so I
relied entirely on CI for binary compilation. The build failure on the first push came directly
from that gap — I could not catch an include-ordering issue without the actual compiler.

If the PR receives review comments, the next step is to respond within the voice guide's rules
(answer the reviewer's specific request in the first sentence) and push any changes in
per-subsystem commits. If the design question about gyro data surfaces in review, I would open
a separate conversation with the maintainer before writing any additional code.
