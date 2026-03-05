---
name: bug-fix
description: 'Use this skill when you need to fix any bugs or unexpected behavior or a desired behavior should happen but not.'
---

## Workflow

You should follow these workflows:

1. Understand the bug or the unexpected behavior. Ask for more context if needed.
2. Create a unit test to reproduce the bug or behavior. THIS UNIT TEST IS EXPECTED TO FAIL. Only continue if the test fails.
3. Investigate the root cause causing this issue.
4. Keep trying to fix the root cause and use the unit test to verify your fix.
5. If you can't make the test pass after 3 attempts, ask for help.

## Completion Criteria

- The new unit test can reproduce the bug by failing before the fix.
- The new unit test can pass after the fix.
- Existing unit tests still pass.
