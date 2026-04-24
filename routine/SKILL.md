---
name: routine
description: Enter plan mode automatically to analyze the user's requirements.
---

Enter plan mode automatically to analyze the user's requirements.

Steps:
1. Analyze what the user wants to accomplish and identify the dynamic parts (things that vary per run)
2. Create a shell script under `${current working directory}/.claude/scripts/` (NOT under $HOME/.claude/scripts) that:
   - Uses `cc-hub run -p` for the dynamic parts, accepting arguments or prompts
   - Is idempotent and safe to re-run
3. If the script requires parameters, prompt the user for them (or populate intelligently from context)
4. Present the script content and parameters to the user and request explicit approval
5. After approval, make the script executable and run it
