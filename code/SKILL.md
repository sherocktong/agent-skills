# `code` skill

Use this skill when the user asks to open files in VS Code (e.g., "open this in code", "open with code", "code .").

## Instructions

1. Identify files mentioned in the current conversation. Look for:
   - File paths explicitly listed by the user
   - Files referenced in code blocks or tool results
   - The current working directory if no specific files are mentioned

2. Open VS Code with the identified files using the `code` CLI:
   - `code <file1> <file2> ...` to open specific files
   - `code .` to open the current directory
   - `code -g <file>:<line>` to open at a specific line (if line number is mentioned)

3. Use the `Bash` tool to run the command. No sandbox escape needed.

4. Confirm to the user which files were opened.
