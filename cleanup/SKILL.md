---
name: cleanup
description: 'Cleanup redundant code, dead code, unnecessary logs. Fix build warnings. Fix format. Use it after changes are made.'
---

This should be used after changes are made or a refactor is done. Use this skill
to cleanup the codebase so that code reviewer can focus on the actual implementation.

## Workflow

You should follow these workflows:

1. Remove any debug logs unless the purpose of this change is to add debug information.
2. Clean up any dead code / unreachable code.
3. Check if there are redundant code. Refactor them if it makes sense.
4. Fix any build warnings / errors. Use the canonical tool of the language. For example, use clippy for cargo.
5. Fix any format issues.

