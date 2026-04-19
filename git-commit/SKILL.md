Commit staged files with an auto-generated commit message.

Steps:
1. Run `git diff --cached` to check for staged changes
   - If no files are staged, tell the user there is nothing to commit and stop
2. Check for local git configuration:
   - Run `git config --local user.name` and `git config --local user.email`
   - If either is not set (empty output), STOP and tell the user:
     "Cannot commit: Local git user.name and/or user.email are not set. Please configure them first with:
     git config --local user.name 'Your Name'
     git config --local user.email 'your.email@example.com'"
3. Run `git diff --cached` and analyze the staged changes
4. Print the git user name and email from `git config user.name` and `git config user.email`
5. Generate a concise commit message that summarizes the changes
6. Commit using the repo's local git user name and email — do NOT add any Co-Authored-By trailer for Claude
7. Confirm the commit to the user
