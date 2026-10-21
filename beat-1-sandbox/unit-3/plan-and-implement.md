# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

christophermayfield

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57#issuecomment-6323507946

Following up on my repro from last week: the root cause is that `_should_skip_file` in `agent/tools/tech_detector.py` checks for `'/node_modules/'` and `'/build/'` with a leading slash, so paths like `node_modules/lib/index.js` (no leading slash) pass through unfiltered. The fix is to drop the leading slash from each skip pattern so the check fires regardless of whether the path starts at the repo root or has a leading component. I'll also remove the `@pytest.mark.xfail` markers from `test_node_modules_excluded` and `test_build_directory_excluded` once those tests pass. No other files need to change.

---

## Your branch

**Branch**

fix/57-tech-detector-vendored-paths

**Evidence**

**Before (from the Unit 2 repro comment, reproduced at commit `f89c06f`):**

```python
from agent.tools.tech_detector import TechDetector
t = TechDetector()
files = ['main.py','core/app.py','node_modules/lib/index.js','node_modules/lib/util.js','node_modules/x/a.js','node_modules/y/b.js','build/bundle.js','build/vendor.js']
print(t.execute({'files': files}).data['primary_language'])
```
Output: `JavaScript`

```
$ pytest tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded \
         tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded -v
tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded XFAIL
tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded XFAIL
2 xfailed in 0.86s
```

**After (branch `fix/57-tech-detector-vendored-paths`):**

```python
from agent.tools.tech_detector import TechDetector
t = TechDetector()
files = ['main.py','core/app.py','node_modules/lib/index.js','node_modules/lib/util.js','node_modules/x/a.js','node_modules/y/b.js','build/bundle.js','build/vendor.js']
print(t.execute({'files': files}).data['primary_language'])
```
Output: `Python`

```
$ pytest tests/unit/test_tech_detector.py -v 2>&1 | tail -5
tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded PASSED
tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded PASSED
27 passed in 0.14s
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (full, 20 packages): agreement 18/20 (bar: 18/20: PASS). Misses: pkg-11 graded accept (gold: reject), pkg-14 graded reject on plan-executable (gold: accept).

Run 2 (partial, 4 packages — pkg-11, pkg-14, pkg-04, pkg-20 after updating the evidence guide): agreement 4/4.

Run 3 (full, 20 packages — confirming run): agreement 20/20 (bar: 18/20: PASS).

**Package analysis**

pkg-11 (source: mikefarah/yq#2782, category: wrong-cause, gold: reject).

My rubric decided: **accept**. Gold label: **reject**.

The gold note reads "plan blames the collect operator; the package's own control (top-level `[.a] | length == 1`) shows collect already synthesizes null outside condition contexts, ruling that diagnosis out."

My rubric's diagnosis-grounded check read the package as passing because the plan's stated cause — "the collect operator `[...]` fails to synthesize a null entry for a missing key" — is consistent with the symptom shown in the repro (select/and/or contexts returning wrong results). The model did not catch that the very first command in the repro, `yq -n '{} | ([.a] | length)'` → `1`, is a control run showing the collect operator working correctly at the top level. A plan that blames collect contradicts this control: the fault is in the condition context (which sets `DontAutoCreate`), not in collect itself.

After adding to the evidence guide: "When the repro contains a control that shows a component working correctly in one context and failing in another, a plan that blames the component itself contradicts the control: the fault is in the differing context, not the component," the check correctly rejected pkg-11 in Run 2.

**Check rationale**

Current wording (from the uploaded rubric.md, diagnosis-grounded check):

> "the diagnosis follows from what the repro evidence actually shows: it does not contradict a control run or ignore an artifact that rules the stated cause out; a plan that picks up the thread's own confirmed diagnosis passes if the repro evidence is consistent with it"

This check was written to catch plans that adopt a plausible-sounding cause without checking it against the repro evidence. The phrase "does not contradict a control run" was chosen specifically because control runs are the most precise signal in a repro: they isolate one variable and show what the behavior looks like with that variable removed. A plan that blames component X for a bug, when the repro's control run shows X working correctly in a different context, is not grounded — the control rules the diagnosis out even if the stated cause sounds reasonable.

**Trade-offs**

The diagnosis-grounded check loosening in Run 2 (adding the "context vs. component" note to the evidence guide) was verified with a canary set of 4 packages in the partial run: pkg-11 (the target miss), pkg-14 (the other miss), pkg-04 (thread-convention, a category the change could touch if a maintainer's diagnosis was now read differently), and pkg-20 (the disclosure package). All four remained correct in Run 2. The full Run 3 confirmed 20/20 with no new flips.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
