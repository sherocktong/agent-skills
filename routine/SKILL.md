---
name: routine
description: Enter plan mode automatically to analyze the user's requirements.
---

Enter plan mode automatically to analyze the user's requirements.

Steps:
1. Analyze what the user wants to accomplish and identify the dynamic parts (things that vary per run)
2. Create a shell script under `.claude/scripts/` that:
   - Uses `cc-hub run -p` for the dynamic parts, accepting arguments or prompts
   - Is idempotent and safe to re-run
3. Present the script content to the user and request explicit approval
4. After approval, make the script executable and run it
