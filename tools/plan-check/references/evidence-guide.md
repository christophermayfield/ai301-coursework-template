# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives:**
- In an eval bundle: the plan's "Diagnosis" section (or the leading paragraph if the plan has no headings). Also the repro evidence block — specifically the steps, the observed artifact, and any control runs. A control run is the key witness: it shows what behavior persists or disappears when one variable is removed, and a diagnosis that contradicts a control run fails.
- In live mode: the plan.md file's diagnosis section; the student's posted repro comment on the issue thread (fetched via `gh issue view`); the issue's thread highlights for maintainer confirmation of a cause.

**What good looks like:**
The stated cause is consistent with every artifact and control run in the repro evidence. Good: "the control run without the flag shows items parse correctly, ruling the tokenizer out." Bad: blaming the tokenizer when the control shows parsing works without the flag. A plan that adopts the thread's own confirmed diagnosis passes when the repro evidence is consistent with it. When the repro contains a control that shows a component working correctly in one context (e.g., top-level collect produces `[null]`) and failing in another (inside select/and/or), a plan that blames the component itself contradicts the control: the fault is in the differing context, not the component.

## Scope

**Where it lives:**
- In an eval bundle: the plan's "Scope" section or in-scope/not-in-scope statements. Also the full list of changes described anywhere in the plan — files, new options, refactors, migrations, UI work.
- In live mode: plan.md's scope section and any additional work described elsewhere in the plan.

**What good looks like:**
One bounded change: the plan names the specific component to fix and explicitly defers anything else with a stated reason. Scope-creep signals: the plan to fix one bug also rewrites a printer, migrates a dependency, adds a new option, adds a settings panel, or adds UI rework — work the issue never requested. Deferring additional work with a stated reason ("not in scope: the tombstone rework; separate concern") passes.

## Executability

**Where it lives:**
- In an eval bundle: the plan's "Approach," "Files," or numbered steps. The key question is whether at least one specific file or code area is named.
- In live mode: plan.md's approach section.

**What good looks like:**
At least one named file, crate, or code area narrow enough for a stranger to open the repo and know where to start. Naming a specific file path (`agent/tools/tech_detector.py`) is the clearest form, but naming a specific crate and its responsibility ("the client attach/reattach path in `zellij-server`'s session connection handling") also passes when the named area is narrow and the approach has a concrete next step. Not good: "investigate and optimize," "poke around the editor code," "recover somewhere," or naming only the whole repo or a broad directory with no further guidance — these leave a stranger unable to start.

## Test plan

**Where it lives:**
- In an eval bundle: the plan's "Test plan" section. Also the repro evidence block, because a decisive test plan re-runs the repro steps and names the expected outcome after the fix.
- In live mode: plan.md's test plan section; the student's repro comment for what the before state looked like.

**What good looks like:**
The test plan names a specific observable: a named test changing from xfail/failing to passing, a specific command that was producing an error now exiting 0, a measured value improving to a stated threshold, or a specific artifact changing. "The two xfail tests pass and xfail markers are removed" is decisive. "Should feel fast," "run the test suite," "nothing else should break," or "starship timings should look much better" are not — they name no observable tied to the fix.

## Honesty

**Where it lives:**
- In an eval bundle: the plan's risks/unknowns section (if present) and any "not in scope" statements. After a build: the ## Deviations section of plan.md.
- In live mode: plan.md's risks/unknowns section; the ## Deviations section after the build is complete.

**What good looks like:**
Unknowns are stated as unknowns: "I haven't measured the benchmark cost yet; if it shows up I will move the check to the two growth sites" is honest. "Optimize whatever shows up" with no named starting point is false confidence dressed as a plan. A deviation recorded in ## Deviations with a stated reason is honest work; a blank Deviations section earns nothing.

## Comms

**Where it lives:**
- In an eval bundle: the candidate plan comment, read against the thread highlights (maintainer signals) and the repo-facts block's contribution policy (especially any AI-use disclosure requirement).
- In live mode: comment.md; the issue's live thread fetched via `gh issue view --json comments`; the repo's CONTRIBUTING.md and any AI_POLICY.md fetched via `gh api`.

**What good looks like:**
The plan comment engages every explicit maintainer action in the thread: if an owner named a file as the culprit, the comment names a fix at that file (or explains why not); if a patch was posted and testing requested, the comment acknowledges it. A comment that proposes a docs-only fix when the owner posted a code patch and asked for testing is thread-unaware. If the repo's stated policy requires AI-use disclosure ("all AI usage must be disclosed"), the comment includes that disclosure; silence in a disclosure-required repo is a comms failure.
