# Plan: fix tech detector not excluding top-level vendored/build directories (issue #57)

## Diagnosis

`_should_skip_file` in `agent/tools/tech_detector.py` checks for path patterns using substring containment with a leading slash (e.g., `'/node_modules/'`). This correctly filters paths like `/project/node_modules/lib/index.js` (which contain `/node_modules/` as a substring), but fails to filter paths that start at the directory root without a leading slash, such as `node_modules/lib/index.js`.

This is grounded directly in the repro evidence:

From the posted repro comment, tracing the skip check:
```python
>>> t._should_skip_file('node_modules/lib/index.js')
False   # not skipped — counted as JavaScript
>>> t._should_skip_file('/project/node_modules/lib/index.js')
True    # skipped correctly when path has a leading component
```

The repro script (exact inputs from the issue) produces `JavaScript` instead of `Python` because 6 JS files under `node_modules/` and `build/` pass through the filter unblocked.

## Scope

**In scope:** Fix `_should_skip_file` so it correctly excludes paths that start with a skip-pattern directory (e.g., `node_modules/lib/index.js`, `build/bundle.js`) as well as paths that contain the pattern in the middle (e.g., `/project/node_modules/lib/index.js`). Remove the `@pytest.mark.xfail` markers from the two now-passing tests.

**Not in scope:** Adding new skip patterns (the existing list covers the needed cases), changing what languages are detected, or modifying any other part of `TechDetector`.

## Files

- `agent/tools/tech_detector.py` — the `_should_skip_file` static method (line 151)
- `tests/unit/test_tech_detector.py` — remove `@pytest.mark.xfail` from `test_node_modules_excluded` and `test_build_directory_excluded`

## Approach

1. In `_should_skip_file`, change each pattern to match at any path-component boundary. The simplest fix is to strip the leading slash from each pattern so it checks for the directory name followed by a slash at any position. Alternatively, add both a leading-slash form and a no-leading-slash form for each pattern. The cleanest approach: replace `'/node_modules/'` with `'node_modules/'` across all patterns (the trailing slash already anchors to a directory name, so `node_modules.js` is not falsely matched).

2. Run `pytest tests/unit/test_tech_detector.py` to confirm `test_node_modules_excluded` and `test_build_directory_excluded` now pass (XPASS with strict=True means the suite fails until the markers are removed).

3. Remove the `@pytest.mark.xfail` decorator lines from both tests.

4. Re-run the full unit test suite to confirm no regressions.

## Test plan

**Before (from repro comment):**
```python
from agent.tools.tech_detector import TechDetector
t = TechDetector()
files = ['main.py','core/app.py','node_modules/lib/index.js','node_modules/lib/util.js','node_modules/x/a.js','node_modules/y/b.js','build/bundle.js','build/vendor.js']
print(t.execute({'files': files}).data['primary_language'])
# observed: 'JavaScript'
```

**After:** The same script should print `Python`.

**Pytest confirmation:** `test_node_modules_excluded` and `test_build_directory_excluded` must change from XFAIL to PASS (not XPASS — the markers will be removed, so the tests simply pass).

```
$ pytest tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded \
         tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded -v
# expected: PASSED PASSED, 2 passed
```

## Risks and unknowns

- Paths passed to `TechDetector` may vary in format (absolute paths with leading `/`, relative paths without). The fix handles both by checking for the pattern without a leading slash, which is a substring of both forms. No known risk of false positives since the patterns all end with `/` (directory boundary).
- No other callers of `_should_skip_file` in the codebase — only used in `_detect_tech`.

## Deviations

The build matched the plan exactly. The approach of removing the leading slash from all skip patterns was the first option listed, and it worked as described. The two xfail tests became XPASS(strict) immediately after the one-line change per pattern, confirming the fix was complete before the markers were removed. No additional files were touched and no new patterns were added.
