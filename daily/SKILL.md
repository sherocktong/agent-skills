---
name: daily
description: Scan all CLAUDE.md files recursively under the current working directory, collect Work Log entries for a specific date (default today), and generate a human-readable daily work report.
---

Scan all CLAUDE.md files recursively under the current working directory, collect Work Log entries for a specific date, and generate a daily work report.

The output is a work report meant for humans — written in plain prose at an outcome level, not a dump of raw entries. Think "what did I get done today" as you'd tell a manager or write in a standup, not a technical changelog.

Steps:
1. Determine the target date:
   - If the user passed a date argument (e.g. `2026-04-27` or `yesterday`), use it
   - Otherwise default to today: `date '+%Y-%m-%d'`
2. Find all CLAUDE.md files recursively from the current working directory:
   - `find . -type f -name 'CLAUDE.md' -not -path '*/node_modules/*' -not -path '*/.git/*'`
3. For each CLAUDE.md file:
   - Locate the `## Work Log` section
   - Extract entries whose timestamp header (`### YYYY-MM-DD ...`) matches the target date
4. Aggregate matching entries across all files. Note the source project/directory of each so you can group by area of work.
5. **If no entries are found for the target date, stop here and tell the user explicitly:**
   - State the date that was searched (e.g. "No work log entries found for 2026-04-27.")
   - Mention how many CLAUDE.md files were scanned, so it's clear the search ran
   - Do NOT generate an empty report or fabricate content
6. Otherwise, generate a consolidated daily work report:
   - Header: `# Daily Work Report — <YYYY-MM-DD>`
   - Write a short prose summary (2-5 sentences) covering the day's themes and overall progress
   - Follow with a concise bullet list of accomplishments, grouped by project/area when multiple are involved
   - Strip out implementation details, file paths, commands, and tool-call noise — merge near-duplicate entries into a single outcome
   - End with a brief "Follow-ups" section only if there are open items worth flagging
7. Output the report to the user in the response
