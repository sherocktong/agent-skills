Manage ComfyUI using `comfy` and `cm-cli` CLIs.

## comfy — ComfyUI lifecycle & models

**Launch & stop:**
- `comfy launch --background` — launch ComfyUI in background
- `comfy stop` — stop background ComfyUI
- `comfy which` — show which ComfyUI workspace is selected
- `comfy env` — print current environment variables

**Run workflows:**
- `comfy run --workflow <api_json> --wait` — run an API workflow file against a background ComfyUI instance
- Use `--host` and `--port` to target a specific instance, `--timeout` to set execution timeout (default 30s)

**Models:**
- `comfy model list` — list downloaded models
- `comfy model download --url <url>` — download a model
- `comfy model remove` — remove downloaded models

**Custom nodes:**
- `comfy node install <node>` — install a custom node
- `comfy node uninstall <node>` — uninstall a custom node
- `comfy node update` — update custom nodes or ComfyUI itself
- `comfy node show` — show installed custom nodes
- `comfy node disable <node>` / `comfy node enable <node>` — toggle nodes
- `comfy node fix <node>` — fix dependencies of a custom node
- `comfy node deps-in-workflow <workflow>` — generate dependencies from a workflow file
- `comfy node install-deps <deps_file_or_workflow>` — install dependencies from file or workflow

**Setup:**
- `comfy install` — download and install ComfyUI + ComfyUI-Manager
- `comfy update` — update ComfyUI environment
- `comfy set-default` — set default ComfyUI path

## cm-cli — ComfyUI-Manager CLI

Used for node management when ComfyUI is not running.

**Node management:**
- `cm-cli install <node>` — install custom node
- `cm-cli uninstall <node>` — uninstall custom node
- `cm-cli reinstall <node>` — reinstall custom node
- `cm-cli update <node>` — update custom node
- `cm-cli enable <node>` / `cm-cli disable <node>` — toggle nodes
- `cm-cli fix <node>` — fix dependencies
- `cm-cli show` / `cm-cli simple-show` — list nodes

**Dependencies:**
- `cm-cli deps-in-workflow <workflow>` — generate dependencies from workflow (.json/.png)
- `cm-cli install-deps <file>` — install dependencies from file or workflow
- `cm-cli restore-dependencies` — restore all dependencies from installed nodes

**Snapshots:**
- `cm-cli save-snapshot` — save environment snapshot
- `cm-cli restore-snapshot <file>` — restore from snapshot

**Other:**
- `cm-cli cli-only-mode` — toggle CLI-only mode (no ComfyUI server needed)
- `cm-cli export-custom-node-ids` — export custom node IDs
