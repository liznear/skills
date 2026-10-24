---
name: orca-handoff
description: Start a named Pi session in a new Orca terminal and hand off the current work through pi-intercom. Available only through explicit invocation.
disable-model-invocation: true
---

# Orca Handoff

Transfer ownership of the current work to a fresh Pi session. Use any arguments supplied with this skill as the next task; otherwise hand off the current unfinished work.

1. Call `intercom({ action: "list" })` and record the original (current) session's ID for the receiver to contact later. Choose a short, meaningful session name related to the handoff task, using only lowercase letters, digits, and hyphens. Append a random suffix and check the roster to ensure uniqueness, for example `handoff-api-fix-a7c2`.

2. From the intended project directory, create one terminal in the current Orca workspace:

   ```sh
   orca terminal create --command "pi --name handoff-api-fix-a7c2" --json
   ```

   Substitute the chosen session name. Use `orca` directly. Retain the returned terminal handle. Use the current checkout unless the user specified another existing workspace; for that case, select it with `--worktree`. Do not create a new worktree as part of this skill.

3. Call `intercom({ action: "list" })` until the newly named session appears live, allowing up to 30 seconds for startup with short pauses between checks. Verify its name, working directory, and session ID, and ensure it is not the current session. Target that session ID for delivery. If it never appears, inspect the created terminal with `orca terminal read --terminal <handle> --json` and report the blocker. Do not launch another session automatically.

4. Send one self-contained handoff using pi-intercom:

   ```js
   intercom({
     action: "send",
     to: "<new-session-id>",
     message: "<handoff instructions>"
   })
   ```

   Include the goal and next concrete action, user constraints and decisions, relevant paths and branch, changes already made (including uncommitted work), verification results, and remaining work or blockers. State that the receiver owns the task and should continue it. Keep the message focused on what the receiver needs to proceed without the original conversation.

   Always include the original session's actual ID and this instruction in the message: "If you need more context later, ask the original session through pi-intercom: `intercom({ action: \"ask\", to: \"<original-session-id>\", message: \"<your context question>\" })`." Substitute the recorded original session ID before sending.

5. Check the delivery result. On success, report the new session name and terminal handle, then stop work on the handed-off task. Do not wait for the receiver to finish. If delivery fails, report the failure and preserve the created session for recovery; do not claim the handoff succeeded or send the instructions through terminal input instead.
