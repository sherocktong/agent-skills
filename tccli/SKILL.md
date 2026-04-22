---
name: tccli
description: # tccli skill
---

# tccli skill

Use the Tencent Cloud CLI (`tccli`) to interact with any Tencent Cloud service on behalf of the user.

## Trigger

Invoke this skill when the user asks to:
- Query, create, update, or delete any Tencent Cloud resource
- Run a `tccli` command
- Work with services like CVM, COS, TKE, VPC, SCF, CLB, CBS, CDB, etc.

## Behavior

1. **Understand the request** — identify the target service and operation from the user's description.
2. **Check credentials** — if `tccli` is not configured, prompt the user to run `tccli configure` first.
3. **Construct the command** — build the correct `tccli` invocation:
   ```
   tccli <service> <action> [--param value ...]
   ```
   Examples:
   - `tccli cvm DescribeInstances --region ap-guangzhou`
   - `tccli cos GetBucket --Bucket my-bucket-1250000000 --Region ap-guangzhou`
   - `tccli tke DescribeClusters`
4. **Execute the command** using the Bash tool.
5. **Parse and present results** — summarize the JSON output in a human-readable way. Show relevant fields; don't dump raw JSON unless the user asks.
6. **Handle errors gracefully** — if the command fails, explain the error and suggest a fix (wrong region, missing permission, typo in resource ID, etc.).

## Command structure reference

```
tccli [options] <service> <action> [--param value ...]
```

Global options:
- `--region <region>` — e.g. `ap-guangzhou`, `ap-beijing`, `ap-singapore`
- `--profile <name>` — use a named credential profile (configured via `tccli configure --profile <name>`)
- `--output [json|table|text]` — output format (default: json)

To list all supported services: `tccli --help`
To list actions for a service: `tccli <service> --help`
To see parameters for an action: `tccli <service> <action> --help`

## Configuration

If credentials are missing or the user needs to set up a new profile:
```bash
tccli configure
# or for a named profile:
tccli configure --profile myprofile
```
Required inputs: SecretId, SecretKey, Region, output format.

## Notes

- Always confirm the target region with the user if it is ambiguous.
- For destructive operations (delete, terminate, stop), ask the user to confirm before executing.
- Prefer `--output json` for programmatic parsing; use `table` for human summaries when appropriate.
- If the user needs to install tccli: `pip install tccli`
