---
name: default-browser
description: Set the macOS default browser using the defaultbrowser CLI. Supports Safari and Google Chrome.
triggers:
  - default browser
  - defaultbrowser
  - set default browser
---

# default-browser skill

You have been invoked to set the macOS default browser using the `defaultbrowser` CLI.

## Prerequisites

Ensure `defaultbrowser` is installed:

```bash
which defaultbrowser
```

If not found, install it (typically via Homebrew):

```bash
brew install defaultbrowser
```

## Supported browsers

| Friendly name | CLI argument |
|---------------|--------------|
| Safari        | safari       |
| Google Chrome | chrome       |

## Workflow

1. **Show the current default browser** (optional but helpful):

   ```bash
   defaults read com.apple.LaunchServices/com.apple.launchservices.secure LSHandlers | \
     grep -B 5 'LSHandlerURLScheme = https' | \
     grep 'LSHandlerRoleAll' | \
     sed 's/.*= "\(.*\)";.*/\1/'
   ```

2. **List available browsers**:

   ```bash
   defaultbrowser
   ```

3. **Ask the user which browser to set** if they haven't specified:

   > Which browser should be the default? Options: Safari, Google Chrome.

4. **Set the default browser**:

   ```bash
   defaultbrowser <short-name>
   ```

   Examples:

   ```bash
   defaultbrowser safari
   defaultbrowser chrome
   ```

5. **Confirm the change**:

   Re-run the `defaults read` command from step 1 to verify the new default is active, then report the result to the user.

## Rules

- Only use the two supported short names listed above unless the user explicitly requests another browser shown in `defaultbrowser` output.
- Always confirm the user's intent before changing the default browser.
- Report the exact command executed and the final state after the change.
