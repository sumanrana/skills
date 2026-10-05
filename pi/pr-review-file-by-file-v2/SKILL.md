---
name: pr-review-file-by-file-v2
description: Reviews a GitHub pull request against stated requirements, one changed code file at a time, with tool checks, progress tracking, explicit finding decisions, and an optional safe HTML record. Use when a user gives a PR number and asks for a strict diff-only review, release-blocker findings, or a file-by-file review artifact.
---

# PR review, file by file, v2

## Inputs and setup

Before starting, require:

1. The GitHub PR number.
2. The product or security requirements.

If either input is missing, ask for it and stop. Do not inspect or review the PR yet. Do not guess requirements from the code.

Before reading the PR, check that GitHub CLI is installed and logged in:

```bash
gh --version
gh auth status
```

If either command fails, stop. Show the error and the exact step needed to fix it. Do not start the review.

Follow repository instructions and maps when present. Source code is the authority. Then run:

```bash
gh pr view <PR> --json number,title,body,baseRefName,headRefName,files,commits
gh pr diff <PR> --name-only
gh pr diff <PR> --color=never
```

Use the PR base from GitHub. Do not assume `main` or the current branch. Inspect every changed file. Review only hunks that change executable code, SQL, configuration logic, or rendered templates. Ignore documentation-only, comment-only, and whitespace-only changes.

## Review loop

1. Keep files in `gh pr diff --name-only` order.
2. Before each file, show:

   **Progress:** File <current> of <total>  
   **Next file:** `<exact path>`

3. Review exactly one file, then stop and wait.
4. Track each file as Approved, Finding accepted, Finding ignored, Skipped with reason, or Not reviewed.
5. Continue until every changed code file is covered.

Handle special files as follows:

- **New file:** Review all added code.
- **Deleted file:** Check whether callers or configuration still use it.
- **Renamed file:** Check path-based imports, routes, templates, and configuration.
- **Large file:** Review it in smaller chunks. Do not skip it because of size.
- **Mixed file:** Review code changes. Ignore documentation-only and whitespace-only changes.
- **Binary or generated file:** Skip it and record the reason.

For each file:

- Read its full diff and enough old and new context to prove behavior.
- Anchor each finding to an added, removed, or edited line. For a pure rename, anchor it to the rename record.
- Use unchanged code only as evidence. Never base a finding only on old code.
- Trace callers, data, errors, and side effects for the changed path.
- Compare with actual requirements, not guessed intent.
- Put a cross-file defect on the changed file that owns the root cause. Avoid duplicates.
- Do not invent defensive failures when the same PR cannot produce that state.

## Finding threshold

Report release blockers by default. Skip style, cleanup, optional hardening, and small UX suggestions unless the user asks for all findings.

A release blocker is a reachable change that can cause:

- security or access-control failure
- data loss or wrong committed state
- rollback failure or a forbidden partial commit
- build or runtime failure on a supported path
- direct failure of a must-have requirement

Risk scores: 9-10 critical security, data, or transaction failure; 7-8 high-impact functional failure; 4-6 medium or limited impact; 1-3 low impact. Confidence is High, Medium, or Low. Use High only when the diff and call path prove the result.

## Response format

Use plain English. Keep paths and code exact. Keep every field:

```markdown
## <number>. <short finding name>
**Title:** <clear title>
**File name:** `<exact path>`
**Risk score:** <1-10>/10
**Severity:** Critical | High | Medium | Low
**Confidence:** High | Medium | Low
**Release-blocker status:** Yes | No

**Code chunk:**
<exact changed code>

**What is wrong:**
<plain explanation>

**Why it matters:**
<user and system impact>

**Suggested fix:**
<smallest safe fix, including other owning files when needed>
```

If no release blocker exists, say:

```markdown
## BLUF
No release blocker found in this file.
**File name:** `<exact path>`
**Review result:** Approved. No code change needed.
```

After each file, wait for one of these replies:

- `accept <finding number>` — keep the finding.
- `ignore <finding number>` — remove the finding.
- `explain <finding number>` — explain it more simply.
- `next` — continue to the next file.
- `stop` — pause the review.

Plain-language replies with the same meaning are valid. Do not continue while a finding still needs a decision.

## Final summary

After all files, show accepted release blockers, ignored findings, approved files, skipped files with reasons, and files not reviewed. Do not mark the PR approved if any code file was not reviewed. State the final result as **Approved**, **Blocked**, or **Incomplete**.

## HTML artifact

Only update the artifact when the user asks. Default path:

```text
code-review/pr-<PR>-code-review-gpt.html
```

Append the exact user question and exact assistant answer. Do not rewrite accepted content. Improve presentation only with HTML headings, tables, bullets, code blocks, responsive CSS, and print CSS.

Before writing HTML:

- Escape user text, assistant text, file paths, and code.
- Treat all copied content as plain text.
- Do not copy raw `<script>` tags, event handlers, or unsafe links into the page.
- Use UTF-8 encoding.
- Validate that the final HTML opens correctly.
