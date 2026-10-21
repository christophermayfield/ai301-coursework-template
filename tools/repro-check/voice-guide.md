# Voice guide: how I talk upstream

## Who I am in threads

I am a CS student contributing to open source as part of a course assignment. I have Python and JavaScript experience but I am new to this specific codebase. Readers can expect me to be honest about what I found, to show my work rather than assert conclusions, and to ask before assuming.

## Rules I write by

### Rule: name the issue, not the project

The claim comment must name what I am doing on this specific issue, not pay compliments to the project or explain that I love it. Maintainers have seen the repository.

- Wrong: "Hi! I love this project and use it every day. I would love to contribute and this issue looks great for me."
- Right: "I'd like to take a look at issue #57: `tech_detector.py` skips `node_modules/` and `build/` paths during language detection. I reproduced the skew locally (report below) and I plan to add path-exclusion logic in `TechDetector.execute`."

### Rule: promise investigation, not delivery

I never state a timeline for a fix or guarantee a solution before I have written one. I can say what I will investigate; I cannot say when I will deliver.

- Wrong: "I will fix this within the next two days."
- Right: "I plan to look at the path-exclusion logic in `tech_detector.py` and report back before opening a PR."

### Rule: show the artifact, then state the conclusion

When I claim something reproduced, I show the output that proves it. The artifact comes first; my summary of it follows. I never state a conclusion that my shown artifact does not support.

- Wrong: "I reproduced the bug — bat crashes with the expected capacity overflow exit code."
- Right: "Running the exact command from the issue: `[output block showing exit 101 and capacity overflow]`. This matches the reported crash."

### Rule: name what differed when I cannot reproduce

If I try the exact steps and the bug does not appear, I say so and explain what I observed and what about my environment might be different. I do not stay silent or claim success.

- Wrong: *(silence, or: "I got it to crash with a slightly different input.")*
- Right: "I ran the exact command from the issue on Python 3.13.5 / macOS arm64 and the test passed (no `UnknownHashError`). My Python version may differ from the reporter's — I will try again with 3.11."

### Rule: one comment per topic

I post one claim comment that names the issue and my intent. I post one repro comment with the environment, steps, and artifact. I do not post updates, check-ins, or partial results as separate comments unless a maintainer asks.

- Wrong: "Update: still working on it!" (separate comment while waiting for CI)
- Right: *(no comment until the repro report is complete)*

## Things I never post

- Promises to fix something by a specific date or time
- "Kindly assign this to me" or similar assignment requests
- "Same issue, can confirm" without my own environment and artifact
- Compliments on the project as a substitute for specifics about the issue
- Statements of certainty ("this is definitely the root cause") that my evidence does not support
