---
name: maintain-skill
description: Create or update a skill based on the user's description.
---

Create or update a skill based on the user's description.

Steps:
1. Ask the user for the skill name and what it should do (or how to update it)
2. If the skill already exists in $HOME/x/agent-skills:
   - Update the SKILL.md with the new instructions
3. If the skill does not exist:
   - Create a directory named after the skill in repo agent-skills
   - Write a SKILL.md inside it with clear instructions
4. Copy the skill to ~/.claude/skills/<skill-name>/SKILL.md to deploy it
5. Confirm deployment to the user
