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
3. **Prompt for profile** — if no `--profile` is mentioned in the user's request or context, ask the user which profile to use before constructing the command.
4. **Construct the command** — build the correct `tccli` invocation:
   ```
   tccli <service> <action> [--param value ...]
   ```
   Examples:
   - `tccli cvm DescribeInstances --region ap-guangzhou`
   - `tccli cos GetBucket --Bucket my-bucket-1250000000 --Region ap-guangzhou`
   - `tccli tke DescribeClusters`
5. **Request approval for updates** — if the action creates, modifies, or deletes any resource, present the command to the user and ask for explicit approval before executing.
6. **Execute the command** using the Bash tool.
7. **Parse and present results** — summarize the JSON output in a human-readable way. Show relevant fields; don't dump raw JSON unless the user asks.
8. **Handle errors gracefully** — if the command fails, explain the error and suggest a fix (wrong region, missing permission, typo in resource ID, etc.).

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

## Endpoint configuration

TCCLI defaults to the nearest endpoint. To set a custom endpoint for a service:
- Default: `tccli configure set cvm.endpoint cvm.ap-guangzhou.tencentcloudapi.com`
- One-time override: `tccli cvm DescribeZones --endpoint cvm.ap-guangzhou.tencentcloudapi.com`

## Notes

- Always confirm the target region with the user if it is ambiguous.
- Always ask the user which profile to use when none is specified.
- For any action that creates, updates, or deletes resources, ask for explicit user approval before executing.
- Prefer `--output json` for programmatic parsing; use `table` for human summaries when appropriate.
- If the user needs to install tccli: `pip install tccli`
- Reference document: https://cloud.tencent.com/document/product/440/60812
