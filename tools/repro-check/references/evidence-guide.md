# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives:**
- In an eval bundle: the repro report section, usually in a line labeled "Environment:" or a leading paragraph before the steps. Also check the issue context for the reported OS/version, and the repo-facts block for the template's stated asks.
- In live mode: the student's draft repro comment; compare against the issue thread's stated environment and the repo-facts block's template requirements.

**What good looks like:**
The report names the tool version (e.g., `bat 0.26.1`, `minikube v1.38.0`) and the OS or platform (e.g., `Fedora 44 x86_64`, `macOS 14.6 arm64`). For platform-specific issues (Windows-only tunnel behavior, GPU backend differences), the relevant driver, backend, or install method is also stated. A version that differs from the issue's target is acceptable if the reporter explicitly calls out the difference ("I used v0.26.1, the issue targets main, here is what I saw").

## Steps

**Where it lives:**
- In an eval bundle: the numbered or sequential steps in the repro report, read alongside the issue's trigger description.
- In live mode: the draft repro comment's steps section; compare against the issue body's "steps to reproduce."

**What good looks like:**
Steps start from a state a stranger can establish (a fresh install, a specific config, a named file). Each step includes the exact command or action (not "run the tool" but `printf 'l1\nl2\n' | bat --no-config --paging=never --line-range ':-N' -`). Steps do not require private resources: a private repo, an unshared config file, or proprietary infrastructure make a step unreproducible. The trigger step — the one that causes the behavior — is present and matches the issue's described trigger, not an adjacent variation. A report that says it ran "the issue's script verbatim" or "the issue's steps exactly" is followable if the issue section of the bundle supplies the complete trigger inputs (the exact options, file contents, or API calls); the issue context is part of the package and a stranger with the bundle can reconstruct the steps.

## Behavior shown

**Where it lives:**
- In an eval bundle: the artifact block(s) in the repro report — code-fenced output, log excerpts, screenshots, or "Expected / Actual" pairs — read against the issue's description of what goes wrong.
- In live mode: the artifact blocks in the draft repro comment; compare against the issue body's description of the failing behavior.

**What good looks like:**
The artifact shows the same thing the issue describes: the same error type (a `capacity overflow` panic, not a graceful argument-validation message), the same exit code, the same wrong output. An adjacent outcome — a related but different error — is not a match. A cannot-reproduce is a match when the report shows a genuine attempt at the trigger and states clearly what was observed instead, and what environmental difference may explain the gap.

## Honesty

**Where it lives:**
- In an eval bundle: the narrative in the repro report's prose, compared to the actual content of its artifact blocks.
- In live mode: compare what the draft says happened against what its shown artifacts actually show.

**What good looks like:**
Every claim of reproduction points to a shown artifact whose content the grader can inspect. "Reproduced and confirmed" backed by an output block showing the issue's exact failure is honest. "Reproduced ten times consistently" backed by no artifacts is not. The report does not narrate the issue's behavior over an artifact that shows something different (e.g., calling a validation error a crash because the exit code is nonzero). If the bug did not reproduce, the report says so, describes what was tried, and states what differed — that is an honest cannot-reproduce and passes.

## Comms

**Where it lives:**
- In an eval bundle: the candidate claim comment and repro report, read against the issue's specifics and the repo-facts block's contribution policy (especially any AI-use disclosure requirement).
- In live mode: the student's draft claim or repro comment; the repo's CONTRIBUTING.md or AI_POLICY.md (fetched via `gh` or the GitHub API); the issue thread.

**What good looks like:**
The claim comment is specific to this issue: it names what the reporter observed or will observe, names a concrete next step (look at a specific file, test a specific fix, run against the latest main), and does not contain interchangeable phrases ("great project," "kindly assign," "will fix in 2 days") that could apply to any issue in any repo. If the repo's stated policy requires disclosing AI usage (e.g., "all AI usage must be disclosed"), the submitted comments include that disclosure; silence on AI use in a disclosure-required repo is a comms failure. A disclosure requirement that is conditional ("if you used AI") passes when the comment either discloses or the context makes clear no AI was used.
