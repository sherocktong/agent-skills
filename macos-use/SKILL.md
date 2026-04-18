Automate macOS tasks using the `macos-use` Gradio app.

## Launching

Run `macos-use` to start the Gradio web interface. It opens on `http://0.0.0.0:7860` (or `SERVER_PORT` env var).

## Using the interface

1. In the **Agent** tab, enter a task prompt describing what the agent should do on macOS
2. Optionally click "Refine Prompt" to let the LLM improve your prompt
3. Click "Run" to execute the task
4. Monitor the terminal output and result

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
