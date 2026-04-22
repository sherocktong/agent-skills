---
name: smart-rename
description: Rename the current session across all contexts.
---

Rename the current session across all contexts.

Auto-generate a smart name by analyzing the conversation context:
- Consider what the user has been working on (bug fix, feature, refactor, review, etc.)
- Include the project or component name if relevant
- Keep it short and descriptive (2-5 words, e.g. "fix login timeout", "add dark mode", "refactor auth middleware")

If the conversation just started with no context, fall back to the current working directory name.

Perform all of the following based on the detected environment:

1. **Claude Code session**: Rename by appending BOTH a `custom-title` record AND an `agent-name` record to the current session's JSONL transcript file. The record formats are:
   ```json
   {"type":"custom-title","customTitle":"<name>","sessionId":"<sessionId>"}
   {"type":"agent-name","agentName":"<name>","sessionId":"<sessionId>"}
   ```
   To find the correct JSONL file:
   - List files in `~/.claude/projects/` sorted by modification time: `ls -lt ~/.claude/projects/*/*.jsonl | head -3`
   - Extract `sessionId` from the first line of the most recently modified JSONL: `head -1 <file> | jq -r '.sessionId'`
   - Append both records:
     ```bash
     echo '{"type":"custom-title","customTitle":"<name>","sessionId":"<sessionId>"}' >> <file>
     echo '{"type":"agent-name","agentName":"<name>","sessionId":"<sessionId>"}' >> <file>
     ```

2. **Terminal title**: Set the terminal title using the generic OSC escape sequence:
   ```bash
   printf '\033]0;%s\007' "<name>"
   ```

3. **Terminal tab/window (emulator-specific)**:
   - **WezTerm** (`$WEZTERM_EXECUTABLE` or `$TERM_PROGRAM` == `WezTerm`): `wezterm cli set-tab-title "<name>"` or `wezterm cli set-window-title "<name>"`
   - **iTerm2** (`$TERM_PROGRAM` == `iTerm.app`): AppleScript:
     ```bash
     osascript -e 'tell application "iTerm2" to tell current session of current window to set name to "<name>"'
     ```
   - **macOS Terminal.app** (`$TERM_PROGRAM` == `Apple_Terminal`): AppleScript:
     ```bash
     osascript -e 'tell application "Terminal" to set custom title of front window to "<name>"'
     ```
   - **Tmux** (`$TMUX` is set): `tmux rename-window "<name>"`
   - **Generic terminals** (Alacritty, Kitty, etc.): `echo -ne '\033];<name>\007'`

Steps:
1. Analyze the conversation to generate a concise, descriptive name
2. Detect the terminal emulator by checking environment variables in order:
   - `$TMUX` → tmux
   - `$WEZTERM_EXECUTABLE` or `$TERM_PROGRAM` == "WezTerm"` → WezTerm
   - `$TERM_PROGRAM` == "iTerm.app"` → iTerm2
   - `$TERM_PROGRAM` == "Apple_Terminal"` → Terminal.app
   - fallback → generic OSC escape sequence
3. Find the current session's JSONL file and append BOTH the `custom-title` AND `agent-name` records to rename the Claude Code session (both are needed for full compatibility)
4. Set the terminal title via the generic OSC escape sequence (always do this as a baseline)
5. Rename the terminal tab/window using the emulator-specific method detected in step 2
6. If in tmux, also rename the tmux window
7. Done — no confirmation needed
