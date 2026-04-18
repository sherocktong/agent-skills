# agent-skills

Claude Code skills repository. Each skill lives in its own directory with a `SKILL.md` file.

## Skills

| Skill | Description |
|---|---|
| `browser-use` | Automate browser tasks using `browser-use` CLI attached to an existing Chrome instance |
| `comfy` | Manage ComfyUI using `comfy` and `cm-cli` CLIs |
| `dig` | Analyze an incident report or requirement description: break it down into structured parts, then identify missing points, gaps, and overlooked concerns |
| `git-commit` | Commit staged files with an auto-generated commit message |
| `macos-use` | Automate macOS tasks using the `macos-use` Gradio app |
| `maintain-skill` | Create or update a skill based on the user's description |
| `markdown` | Respond to the user's request in markdown format and copy the response to the clipboard |
| `nohup` | When invoked, run ALL background tasks detached from the Claude Code session using nohup, so they survive even if the session ends |
| `plain-text` | Respond to the user's request in plain text and copy the response to the clipboard |
| `routine` | Enter plan mode automatically to analyze the user's requirements |
| `smart-rename` | Rename the current session across all contexts |
| `smart-review` | Review the current branch's changes for completeness and correctness, then run relevant tests |

## Install a skill

Copy the skill directory to `~/.claude/skills/`:

```bash
cp -r <skill-name> ~/.claude/skills/<skill-name>
```

For example, to install the `git-commit` skill:

```bash
cp -r git-commit ~/.claude/skills/git-commit
```

To install all skills at once:

```bash
for skill in */; do cp -r "$skill" ~/.claude/skills/"$skill"; done
```

Skills are activated immediately after copying — no restart needed.

## Create a new skill

Use the `maintain-skill` skill (`/maintain-skill`) or manually:

1. Create a directory named after the skill in this repo
2. Write a `SKILL.md` inside it with instructions
3. Deploy it: `cp -r <skill-name> ~/.claude/skills/<skill-name>`
