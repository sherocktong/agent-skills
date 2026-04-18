Review the current branch's changes for completeness and correctness, then run relevant tests.

Steps:
1. Run `git diff` and `git diff --cached` to see all changed files on the current branch
2. For each changed file, review:
   - Are there any missing edge cases, error handling, or validation?
   - Are there any TODOs, FIXMEs, or placeholder code left behind?
   - Are there any unused imports, variables, or dead code?
   - Does the logic match the intent described in recent commit messages?
3. Check if tests exist for the changed code:
   - If tests exist, run them and report results
   - If tests are missing for new/changed logic, flag it
4. Report findings as a concise checklist:
   - Issues found (with file:line references)
   - Tests run and their results
   - Missing test coverage
