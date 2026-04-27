---
name: record
description: Record a work log entry summarizing the current session into the Work Log section of the current working directory's CLAUDE.md, written for a technical audience.
---

Record a work log entry summarizing the current session into the Work Log section of the current working directory's CLAUDE.md.

The entry should be readable by technical people (engineers, developers, technical leads). Include enough technical context to be meaningful — what systems, components, languages, or tools were involved, and what key technical decisions or outcomes happened. Skip raw implementation minutiae like full file paths, exact commands, or step-by-step tool call logs.

Steps:
1. Determine the target file: `${current working directory}/CLAUDE.md`
   - If the file does not exist, create it with a top-level title
2. Check whether a `## Work Log` section exists in the file
   - If not, append a new `## Work Log` section at the end of the file
3. Get the current timestamp using `date '+%Y-%m-%d %H:%M:%S'`
4. Summarize the session as a concise technical work log:
   - Focus on outcomes and key technical context, not every granular step
   - Mention relevant technologies, services, components, or architectural areas involved
   - Note significant technical decisions, trade-offs, or design choices made
   - 2-5 bullet points is typical; group closely related items together
   - Avoid: full file paths, shell commands, code snippets, raw tool-call transcripts
   - Include: the goal, the result, the technical scope (e.g. "API layer", "K8s deployment", "React frontend"), and any meaningful follow-ups
5. Append the entry under `## Work Log` in this format:

   ```
   ### YYYY-MM-DD HH:MM:SS
   - <technical outcome / context 1>
   - <technical outcome / context 2>
   ```

   - Newest entries go at the bottom (chronological order)
   - Use `Edit` to insert under the section header rather than rewriting the file
6. Confirm to the user that the entry has been recorded, showing the timestamp used
