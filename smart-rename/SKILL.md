Rename the current session across all contexts.

Given a name argument, perform all of the following:

1. **Claude Code session**: Run `/rename <name>` to rename the current Claude Code session.
2. **Terminal session**: Set the terminal title using the appropriate escape sequence: `printf '\033]0;%s\007' "<name>"`
3. **Tmux window**: If the `$TMUX` environment variable is set, run `tmux rename-window "<name>"` to rename the current tmux window.

Steps:
1. Accept the desired name from the user
2. Rename the Claude Code session via `/rename <name>`
3. Rename the terminal title via the escape sequence
4. If in tmux, rename the tmux window
5. Confirm the rename to the user
