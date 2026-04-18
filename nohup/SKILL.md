When invoked, run ALL background tasks detached from the Claude Code session using nohup, so they survive even if the session ends.

If there are existing background tasks running:
1. Gather all running background tasks via the task list or `ps`
2. For each background task, capture its command or description
3. Stop all background tasks using TaskStop
4. Ensure the log directory exists: `mkdir -p ~/.claude/nohup-logs`
5. Re-run each task's command with `nohup ... &` so it detaches, redirecting stdout/stderr to `~/.claude/nohup-logs/<description>.<PID>.log` (use the nohup background PID in the filename)
6. Report to the user which tasks were re-launched, their PIDs, and log locations

If starting NEW background tasks (do NOT stop or touch any existing tasks):
1. Ensure the log directory exists: `mkdir -p ~/.claude/nohup-logs`
2. Run each command directly with `nohup ... &`, redirecting stdout/stderr to `~/.claude/nohup-logs/<description>.<PID>.log`
3. Capture and report the PID and log path for each task
