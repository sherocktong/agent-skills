---
name: record
description: Record a work log entry summarizing the current session into the Work Log section of the current working directory's CLAUDE.md, written as a human-readable work report (not technical details).
---

Record a work log entry summarizing the current session into the Work Log section of the current working directory's CLAUDE.md.

The entry is a work report meant for humans to read — describe what was accomplished at a meaningful, outcome-oriented level. Skip implementation details, file paths, command outputs, and step-by-step actions.

Steps:
1. Determine the target file: `${current working directory}/CLAUDE.md`
   - If the file does not exist, create it with a top-level title
2. Check whether a `## Work Log` section exists in the file
   - If not, append a new `## Work Log` section at the end of the file
3. Get the current timestamp using `date '+%Y-%m-%d %H:%M:%S'`
4. Summarize the session as a brief work report:
   - Focus on what was accomplished, not how
   - Use plain language a non-engineer or future-self could understand at a glance
   - 1-3 short bullet points is typical; combine related steps into one outcome
   - Avoid: file paths, commands, code snippets, granular tool call lists, raw decisions about implementation
   - Include: the goal, the result, and any meaningful follow-up
5. Append the entry under `## Work Log` in this format:

   ```
   ### YYYY-MM-DD HH:MM:SS
   - <high-level outcome 1>
   - <high-level outcome 2>
   ```

   - Newest entries go at the bottom (chronological order)
   - Use `Edit` to insert under the section header rather than rewriting the file
6. Confirm to the user that the entry has been recorded, showing the timestamp used
