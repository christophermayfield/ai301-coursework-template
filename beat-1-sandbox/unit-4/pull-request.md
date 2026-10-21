# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/236

**Branch**

fix/57-tech-detector-vendored-paths

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (full, 20 packages): agreement 20/20 (bar: 18/20: PASS). All categories matched on the first run.

**Package analysis**

pkg-12 (source: sharkdp/bat#3845, category: unreviewable, gold: reject).

My rubric decided: **reject** on diff-clean. Gold label: **reject**.

The gold note reads "the saturating clamp fix is right and evidenced, but the diff carries a commented-out debug line, a commented-out first attempt, a dead _unused_buffer_probe fn, a TODO on an import, and a re-indent churn hunk; commits are 'wip'/'fix'/'fmt + cleanup'."

My rubric's diff-clean check correctly rejected this package because the diff contains multiple debris items: `// eprintln!("DBG buffer_size = {}", buffer_size);` (a commented-out debug print), a multi-line commented-out first attempt (`// let mut buffered_lines = VecDeque::with_capacity(buffer_size.min(MAX_PREALLOC));` with a comment explaining why it was abandoned), a dead function `fn _unused_buffer_probe(cap: usize) -> usize` that was used to bisect the abort threshold and left in, a `TODO` comment on an import line, and a re-indentation hunk in the read loop that buries the fix in whitespace noise. The commit messages `wip`, `fix`, and `fmt + cleanup` are opaque one-word labels that describe no specific change. The diff-clean check fails on any of these: the first one found in the scan is the commented-out debug line on the `eprintln!` line, which the check quotes as evidence.

**Check rationale**

Current wording (from the uploaded rubric.md, diff-clean check):

> "the diff contains no debug prints, commented-out code blocks, dead functions, re-indentation churn, or edits to files the plan does not name; commits are not labeled wip, fix, or fmt (words that hide what a commit actually changes); a one-line debug leftover counts as debris"

This check was written to catch the unreviewable category, where debris buries the real change. The explicit list — debug prints, commented-out code, dead functions, churn, unlisted files — was chosen because each type makes a reviewer's job harder in a distinct way: debug prints mean the author didn't clean up, commented-out blocks mean alternatives were left in the diff for someone else to decide, dead functions mean tested-then-abandoned code is shipping, and churn means the fix is hard to locate in the noise. The phrase "a one-line debug leftover counts as debris" was added explicitly because a single commented-out `eprintln!` is exactly the kind of debris a reviewer might overlook or dismiss as minor — but the check should not, because the honest answer is that the code was not cleaned up before submission.

**Trade-offs**

The diff-clean check as written could miss debris in commit messages alone if the diff itself were clean (since the procedure says "do not fail on commit style alone unless the rubric adds that"). A PR with a clean diff but `wip` commits would pass this check even though the commit history is noisy. I accept this trade-off because the diff content is what a reviewer reads, and a clean diff with a messy commit log is a style issue, not a correctness one. To verify this doesn't affect any other scored package: the four unreviewable packages (pkg-12, pkg-15, pkg-18, and one other) all have visible diff debris — commented-out blocks, debug eprintln!s, or dead code — that the check catches directly from the diff without needing to rely on commit messages. No scored package passes or fails solely on commit message style.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
