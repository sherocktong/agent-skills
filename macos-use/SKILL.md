---
name: macos-use
description: Automate macOS tasks using the `macos-use` Gradio app.
---

Automate macOS tasks using the `macos-use` Gradio app.

## Launching

Run `macos-use` to start the Gradio web interface. It opens on `http://0.0.0.0:7860` (or `SERVER_PORT` env var).

## Permission check (REQUIRED before running tasks)

Before submitting any task, verify macOS accessibility permissions are granted:

```bash
python3 - <<'EOF'
import subprocess
result = subprocess.run(
    ['osascript', '-e', 'tell application "System Events" to get name of first process'],
    capture_output=True, text=True
)
if result.returncode != 0 or result.stderr:
    print("PERMISSION_MISSING")
else:
    print("OK")
EOF
```

If the output is `PERMISSION_MISSING`:
1. **Stop immediately** — do not proceed with the task
2. Tell the user: "macOS accessibility permissions are not granted. Please go to **System Settings → Privacy & Security → Accessibility** and enable access for Terminal (or your terminal app). Then re-run the task."
3. Wait for the user to confirm permissions are granted before retrying

## Using the interface

1. In the **Agent** tab, enter a task prompt describing what the agent should do on macOS
2. Optionally click "Refine Prompt" to let the LLM improve your prompt
3. Click "Run" to execute the task
4. Monitor the terminal output and result

## Auto-shutdown after task completion

After the task completes (success or failure), shut down the `macos-use` service automatically:

```bash
pkill -f "macos-use" 2>/dev/null || true
# Also kill any process on the port
lsof -ti:7860 | xargs kill -9 2>/dev/null || true
```

Confirm shutdown to the user: "macos-use service has been stopped."

## Task prompts

- Be specific: "open the Calculator app" rather than "open calculator"
- For browsers: "open a new window" before navigating
- Break multi-step tasks into clear instructions
- The agent can: open apps, click elements, type text, create AppleScripts

## Automations tab

Use the Automations tab to create reusable workflows with multiple agents chained together.

## Configuration

In the **Configuration** tab, select the LLM provider (OpenAI, Anthropic, or Gemini) and set the API key. Supported providers require the corresponding env var (`OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, or `GEMINI_API_KEY`).

## Warning

macOS-use interacts with ALL apps and UI components on your Mac. Never run it unsupervised. It can access credentials, auth services, and stored passwords.
