Automate browser tasks using `browser-use` CLI attached to an existing Chrome instance.

## Connection

Always attach to an already-running Chrome instance. Never create a new one.

1. Get the CDP URL: `curl -s http://127.0.0.1:9222/json/version | jq -r '.webSocketDebuggerUrl'`
2. Pass it via `--cdp-url <url>` on every command

## Session Management

- Always name sessions with meaningful names using `--session <name>`
- Before navigation, run `browser-use --cdp-url <url> sessions` to list existing tabs and check if the target is already open
- Always open a new tab for each navigation using `browser-use --cdp-url <url> --session <name> open <url>`
- Always close the session when done: `browser-use --cdp-url <url> --session <name> close`

## Workflow

1. Get CDP URL from `http://127.0.0.1:9222/json/version`
2. List existing sessions to check current state
3. Open a new named session and navigate to the target URL
4. Perform the requested browser actions (click, type, extract, etc.)
5. Close the session when finished
