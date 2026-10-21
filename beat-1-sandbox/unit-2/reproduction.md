# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

christophermayfield

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57#issuecomment-6322218882

I'd like to investigate issue #57 as a first contribution here. I reproduced the language skew locally using the issue's exact file list on Python 3.12.4 / macOS arm64: `TechDetector` reports `JavaScript` instead of `Python` because `_should_skip_file` in `agent/tools/tech_detector.py` checks for `/node_modules/` (with a leading slash), so paths like `node_modules/lib/index.js` (no leading slash) are not filtered out. The two `xfail` tests in `tests/unit/test_tech_detector.py` (`test_node_modules_excluded`, `test_build_directory_excluded`) both confirm the failure (full repro report below). My plan is to fix the skip patterns to match path components at any position — for example by checking for `node_modules/` and `build/` without requiring the leading slash — and then remove the `xfail` markers from those two tests.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57#issuecomment-6322255526

**Environment:** Python 3.12.4, macOS 15.0 arm64, repo at commit `f89c06f` (main), fresh virtualenv (`python3 -m venv .venv && pip install -e ".[dev]"`).

**Steps:**

1. From the repo root with the virtualenv active, run the issue's exact script:

```python
from agent.tools.tech_detector import TechDetector
t = TechDetector()
files = [
    'main.py', 'core/app.py',
    'node_modules/lib/index.js', 'node_modules/lib/util.js',
    'node_modules/x/a.js', 'node_modules/y/b.js',
    'build/bundle.js', 'build/vendor.js',
]
print(t.execute({'files': files}).data['primary_language'])
```

**Observed:**

```
JavaScript
```

**Expected:** `Python` (2 Python source files, 6 vendored/bundled JS files that should be excluded).

**Root cause:** `_should_skip_file` in `agent/tools/tech_detector.py` (line 151) checks for `'/node_modules/'` and `'/build/'` using substring containment. Paths that start at the directory root — like `node_modules/lib/index.js` — do not contain a leading `/`, so the check returns `False` and those files are counted. I confirmed this directly:

```python
>>> t._should_skip_file('node_modules/lib/index.js')
False   # not skipped — counted as JavaScript
>>> t._should_skip_file('/project/node_modules/lib/index.js')
True    # skipped correctly when path has a leading component
```

**Pytest confirmation** (`strict=True` xfail tests confirm the failure):

```
$ pytest tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded \
         tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded -v

tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded XFAIL
tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded XFAIL

2 xfailed in 0.86s
```

Both tests produce `XFAIL` (the assertion fails as expected under the bug), matching the issue. My next step is to fix `_should_skip_file` to strip any leading slash or check for the pattern at any path-component boundary, then confirm both tests become `XPASS` and remove the `xfail` markers.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (partial, 3 packages — pkg-01, pkg-02, pkg-03): agreement 3/3 — smoke test confirming the harness ran under `env -u CLAUDECODE`.

Run 2 (full, 20 packages): agreement 19/20 (bar: 18/20: PASS). Miss: pkg-12 graded reject on steps-followable (gold: accept). The repro report said it ran "the issue's script verbatim" but did not inline the script, and the evidence guide did not yet say that referencing the issue's own script is followable when the issue context contains those steps.

Run 3 (partial, 5 packages — pkg-12, pkg-06, pkg-18, pkg-19, pkg-20 after adding the "issue's own script" clarification to the evidence guide): agreement 5/5.

Run 4 (full, 20 packages — confirming run): agreement 20/20 (bar: 18/20: PASS).

**Package analysis**

pkg-12 (source: prettier/prettier#19795, category: clear-accept, gold: accept).

My rubric decided: **reject** on steps-followable. Gold label: **accept**.

My rubric's steps-followable check read the package as failing because the repro report said "ran the issue's script verbatim in an empty directory: `$ node repro.mjs` with `repro.mjs` containing the issue's two `prettier.format` calls" — the file content was not inlined in the repro text itself, only referenced. My evidence guide's Steps section said steps must "include every command or action needed to hit the behavior," and my grader read the absent `repro.mjs` content as a missing step.

What the rubric missed was that the issue section of the bundle contains the exact inputs (shape A: rangeStart 19, rangeEnd 32; shape B: rangeStart 18, rangeEnd 31; both `parser: "babel"`) needed to reconstruct `repro.mjs`. A stranger with the bundle can build the file from the issue context. After adding "A report that says it ran 'the issue's script verbatim' or 'the issue's steps exactly' is followable if the issue section of the bundle supplies the complete trigger inputs" to the Steps section of the evidence guide, the check correctly passed pkg-12.

**Check rationale**

Current wording (from the uploaded rubric.md, steps-followable check):

> "steps describe a starting state a stranger can reach, include every command or action needed to hit the behavior, and do not require access to private resources (private repos, unshared configs, proprietary infra); omitting a step whose absence changes whether the trigger fires is a fail"

This check was written to catch the minikube-style failure (pkg-06): a Windows-specific issue with no OS, no driver, and a step that omits the `--driver vmware` flag, which is precisely the step whose absence changes whether the behavior fires on Windows vs. other platforms. The key phrase "omitting a step whose absence changes whether the trigger fires is a fail" was added explicitly to rule out "the step just wasn't named, but a stranger could infer it." The minikube case can't be inferred — the driver is a binary choice that determines whether the Windows route-parsing path runs at all.

**Trade-offs**

The steps-followable check as written (before the evidence guide clarification) was too strict: it required all steps to be stated inline in the report, not by reference to the issue. The clarification in the evidence guide — "referencing the issue's own script is followable when the issue context contains those steps" — loosens this, and any package that references the issue's own steps rather than inlining them would now pass where it might previously fail.

To verify the loosening did not flip any already-agreeing package in the unfollowable-comms category (the category most likely to be affected), I included pkg-06, pkg-18, and pkg-19 in the Run 3 canary set alongside pkg-12. All three remained reject (gold: reject), confirming the change did not flip them. pkg-20 (the sole disclosure package) was also included as a canary and remained reject. The full Run 4 confirmed 20/20 with no new flips.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
