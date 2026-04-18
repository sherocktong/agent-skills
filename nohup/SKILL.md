Stop all background tasks in the current Claude session, then re-run them with nohup so they detach and survive session disconnect.

Steps:
1. Gather all running background tasks using Bash with `ps` or check task list for tasks with status `in_progress` that were started via `run_in_background`
2. For each background task, capture its command or description
3. Stop all background tasks using TaskStop
4. Ensure the log directory exists: `mkdir -p ~/.claude/nohup-logs`
5. Re-run each task's command with `nohup ... &` so it detaches from the session, redirecting stdout/stderr to `~/.claude/nohup-logs/<description>.<PID>.log` (use the nohup background PID in the filename)
6. Report to the user which tasks were re-launched with nohup, their PIDs, and where their logs are
