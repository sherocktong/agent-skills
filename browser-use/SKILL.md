---
name: browser-use
description: Automate browser tasks using `browser-use` CLI attached to an existing Chrome instance.
---

Automate browser tasks using `browser-use` CLI attached to an existing Chrome instance.

## Connection

Always attach to an already-running Chrome instance. Never create a new one.

1. Get the CDP URL: `curl -s http://127.0.0.1:9222/json/version | jq -r '.webSocketDebuggerUrl'`
   - If this fails or returns empty, **STOP** and ask the user to check if Chrome is running with `--remote-debugging-port=9222`
2. Pass it via `--cdp-url <url>` on every command

## Session Management

- Always name sessions with meaningful names using `--session <name>`
- Before navigation, run `browser-use --cdp-url <url> sessions` to list existing tabs and check if the target is already open
- Always open a new tab for each navigation using `browser-use --cdp-url <url> --session <name> open <url>`
- Always close the session when done: `browser-use --cdp-url <url> --session <name> close`

## Downloads

Files downloaded by the browser land in `/tmp/browser-use-downloads-{id}/` where `{id}` is a random 8-character hex string generated per browser profile instance — **not** a session name or any other predictable value. To find downloaded files after an action, list all matching directories:

```bash
ls /tmp/browser-use-downloads-*/
```

The most recently modified directory is usually the one for the current session.

## Workflow

1. Get CDP URL from `http://127.0.0.1:9222/json/version`
2. List existing sessions to check current state
3. Open a new named session and navigate to the target URL
4. Perform the requested browser actions (click, type, extract, etc.)
5. Check `/tmp/browser-use-downloads-*/` for any downloaded files
6. Close the session when finished
