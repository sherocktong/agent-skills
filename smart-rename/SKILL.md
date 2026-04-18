Rename the current session across all contexts.

Auto-generate a smart name by analyzing the conversation context:
- Consider what the user has been working on (bug fix, feature, refactor, review, etc.)
- Include the project or component name if relevant
- Keep it short and descriptive (2-5 words, e.g. "fix login timeout", "add dark mode", "refactor auth middleware")

If the conversation just started with no context, fall back to the current working directory name.

Perform all of the following:

1. **Claude Code session**: Run `/rename <name>` to rename the current Claude Code session.
2. **Terminal title**: Set the terminal title using the appropriate escape sequence: `printf '\033]0;%s\007' "<name>"`
3. **Terminal session**: Run `echo -ne '\033];<name>\007'` to set the terminal session name via OSC escape sequence.
4. **Tmux window**: If the `$TMUX` environment variable is set, run `tmux rename-window "<name>"` to rename the current tmux window.

Steps:
1. Analyze the conversation to generate a concise, descriptive name
2. Rename the Claude Code session via `/rename <name>`
3. Rename the terminal title via the escape sequence
4. Rename the terminal session via `echo -ne '\033];<name>\007'`
5. If in tmux, rename the tmux window
6. Done — no confirmation needed
