---
name: record
description: Record a work log entry summarizing the current session into the Work Log section of the current working directory's CLAUDE.md, written for a technical audience. Includes agent session IDs when relevant.
---

Record a work log entry summarizing the current session into the Work Log section of the current working directory's CLAUDE.md.

The entry should be readable by technical people (engineers, developers, technical leads). Include enough technical context to be meaningful — what systems, components, languages, or tools were involved, and what key technical decisions or outcomes happened. Skip raw implementation minutiae like full file paths, exact commands, or step-by-step tool call logs.

Steps:
1. Determine the target file: `${current working directory}/CLAUDE.md`
   - If the file does not exist, create it with a top-level title
2. Check whether a `## Work Log` section exists in the file
   - If not, append a new `## Work Log` section at the end of the file
3. Get the current timestamp using `date '+%Y-%m-%d %H:%M:%S'`
4. Get the current Claude Code session ID (if available) using:
   ```
   python3 -c "import sys,json; print(json.load(open(sys.argv[1]))['sessionId'])" "$HOME/.claude/sessions/$PPID.json" 2>/dev/null
   ```
   - If the command fails or returns nothing, skip including the session ID
5. Summarize the session as a concise technical work log:
   - Focus on outcomes and key technical context, not every granular step
   - Mention relevant technologies, services, components, or architectural areas involved
   - Note significant technical decisions, trade-offs, or design choices made
   - 2-5 bullet points is typical; group closely related items together
   - Include ticket / task references when applicable (e.g., Jira `PROJ-123`, GitHub `#456`, Linear `TEAM-789`). List them inline with the relevant bullet or as a separate `- Refs: TICKET-1, #2` line.
   - Always include the current session ID as a separate `- Session: <session-id>` line if it was successfully retrieved in step 4. If multiple sessions were involved, list them as `- Sessions: <id1>, <id2>`.
   - Avoid: full file paths, shell commands, code snippets, raw tool-call transcripts
   - Include: the goal, the result, the technical scope (e.g. "API layer", "K8s deployment", "React frontend"), and any meaningful follow-ups
6. Check whether an entry already exists in `## Work Log` that:
   - Has a `###` timestamp header with today's date (`YYYY-MM-DD`)
   - Contains the current session ID (from step 4) in its body
   - If such an entry exists, replace the entire entry block (from its `###` line up to, but not including, the next `###` line or end of section) with the newly generated entry using the current timestamp.
   - If no matching entry exists, append the new entry at the bottom of the `## Work Log` section (chronological order).

   Use `Edit` to perform the replacement or insertion rather than rewriting the whole file.

   Entry format:
   ```
   ### YYYY-MM-DD HH:MM:SS
   - <technical outcome / context 1>
   - <technical outcome / context 2>
   ```
7. Confirm to the user that the entry has been recorded or updated, showing the timestamp used
