---
name: review
description: Review a PR or code changes.
allowed-tools: Bash(gh pr checks *), Bash(gh pr view *), Bash(gh pr diff *)
---

Perform a code review for a PR or set of changes.

# Review Goals

1. Catch bugs and edge cases
2. Improve code quality
3. Share knowledge across team

# Review Workflow

## Step 1: Fetch Changes Information

Use `git` or `gh` commands to get the changes.

1. Get a list of changed files in the PR. If changes are not yet in a PR, unless
   specified, get the diff with main branch.
2. Get full diff of changes.
3. Fetch associated issue information in commit messages when applicable to
   understand the root cause or business requirements.

## Step 2: Analyze Changes

Read through the diff systematically:

1. Identify the purpose of the change from the COMMIT file in root directory.
2. Assess the "blast radius" - identify which core components are affected, and
   if the changes require extra review.
3. Group changes by type (code, tests, config, docs) for better deep review
   later.
4. Check the change size. >500 lines of code changes (without tests and docs)
   should be broken into smaller PRs if possible, especially when containing
   unrelated changes.

## Step 3: Deep Review

Perform thorough line-by-line analysis using the review checklist covering:

### Architecture & System Design

1. The code changes should follow the architectural decision in a linked doc
   (e.g., RFC in Quip) if one exists.
2. Backward compatibility - database migration or API changes should remain
   backwards compatible unless unavoidable. For database migrations, follow the
   practices in https://gh.apple.com/tsa/guides/blob/main/migrations.md.
3. Dependencies - Examine any newly introduced third-party libraries, ensuring
   they are necessary and well-maintained.

### Correctness

1. The code achieves its stated purpose without bugs or logical errors.
2. Identify potential edge cases and error conditions, see if those are handled
   well.
3. Watch out for race conditions, thread-safety issues, or memory leaks.

### Maintainability

1. Match existing patterns - code follows architectural patterns already in the
   codebase.
2. Simplicity - Look for ways to simplify the implementation. Avoid
   over-engineering.
3. Readability - Variable and function names convey intent, and the code is
   well-commented where necessary.
4. Documentation - New HTTP APIs should have documentation with it, and complex
   logic should be explained.

### Testing

1. Tests exist - New functionality has corresponding tests, covering happy path,
   edge cases and error conditions.

### Performance

1. Watch out for missing database indexes, memory leaks, etc.
2. Resource intensive operations should be appropriately optimized or batched.

### Observability

1. Ensure the code includes appropriate logging, metrics, or tracing to debug
   issues in production.
2. Errors should be wrapped or logged with enough context to be actionable.

### Security

1. Watch for common vulnerabilities (e.g., SQL injection, hardcoded secrets).

## Step 4: Provide Feedback

Provide detailed review comment on the specific line(s) of code. The feedback
comment should:

- Be specific and actionable
- Explain why a change is requested
- Be focused on the code, not the person
- Use professional, supportive language without excessive flattery

Prefix each comment with the following Emoji to indicate the importance of a
comment at first glance.

- ❓: The reviewer needs an answer to this question to decide if
  they approve the PR or not.
- 🐞: A bug that the reviewer thinks would prevent the code from
  working correctly.
- 💅: A nitpick/suggestion that the reviewer doesn't consider
  critical. If the PR author chooses to ignore such a comment, they should
  explain it in a comment.
- ⚠️: A change request the reviewer considers critical before
  giving their approval.
- 🎋: A wishlist item. Something the reviewer thinks would be
  cool, that's not a blocker.
- 👍: A good practice. Something the reviewer thinks the author has done
  well.

Never submit review comments to github automatically. Never write the review comments to a file.

## Step 5: Add Test Cases to Reproduce Bugs

If you found any bugs, you should add a test case to reproduce this bug. This
helps the users to verify the bug, verify the fix later, and make sure there won't be regression.

DO NOT commit the changes.
