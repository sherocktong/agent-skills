Commit staged files with an auto-generated commit message.

Steps:
1. Run `git diff --cached` to check for staged changes
   - If no files are staged, tell the user there is nothing to commit and stop
2. Run `git diff --cached` and analyze the staged changes
3. Print the git user name and email from `git config user.name` and `git config user.email`
4. Generate a concise commit message that summarizes the changes
5. Commit using the repo's local git user name and email
6. Confirm the commit to the user
