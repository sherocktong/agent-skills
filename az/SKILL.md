---
name: az
description: Use the Azure CLI (`az`) to manage Azure resources. Always confirm subscription before running; update operations require user approval.
triggers:
  - az
  - azure
  - azure cli
---

# az skill

You have been invoked to help the user interact with Azure using the `az` CLI.

## Step 1: Confirm subscription

Every `az` command runs against a specific subscription. Before doing anything else:

1. If the user explicitly provided a subscription ID (or name) in their request, use it.
2. Otherwise, **list available subscriptions and stop to ask the user**:

   First run:

   ```bash
   az account list --output table --query "[].{Name:name, SubscriptionId:id, TenantId:tenantId, IsDefault:isDefault, State:state}"
   ```

   If the command fails because the user is not logged in, follow Step 2 (Confirm login) first, then re-run it.

   Then show the table to the user and ask:

   > Which Azure subscription should I operate under? Please pick one from the list above (provide a subscription ID or name).

Do not proceed until a subscription is confirmed.

Once known, scope every command with `--subscription <id-or-name>`. Prefer this over `az account set`, since it avoids changing the user's global default subscription.

## Step 2: Confirm login

Verify the user is authenticated:

```bash
az account show --subscription <id-or-name>
```

If this fails with an authentication error, stop and tell the user:

> Not signed in to Azure. Please run `az login` (or `az login --use-device-code` in headless environments) and try again.

## Step 3: Classify the operation

Determine whether the user's request is a **read** or **update** operation.

### Read operations (allowed without approval)

These are safe and can be run immediately:
- `az ... list`
- `az ... show`
- `az ... get-*`
- `az account ...` (when only viewing)
- `az resource list / show`
- `az group list / show / exists`
- `az version`
- `az ... export` (when writing to a local file the user requested)
- `az monitor metrics list / log-analytics query`
- `az ... check-*` (e.g. `check-name`)
- Any `--help` invocation

### Update operations (require user approval before running)

These create, modify, or delete Azure resources — always confirm with the user first:
- `az ... create`
- `az ... update`
- `az ... set` / `az ... add` / `az ... remove`
- `az ... delete` / `az ... purge`
- `az ... start` / `az ... stop` / `az ... restart` / `az ... deallocate`
- `az ... assign` / `az ... grant` / `az ... revoke` (RBAC, role assignments)
- `az deployment ... create` / `what-if`-followed-by-create
- `az policy ... create / assign`
- `az ... import` (when it writes to Azure)
- `az login` / `az logout` / `az account set` (changes session/global state — confirm intent)
- Any other command that writes to or modifies Azure

For update operations, show the exact command you plan to run and ask:

> This command will modify Azure resources in subscription `<id-or-name>`. Do you want to proceed?
> ```
> az <command>
> ```

Only run the command after explicit user confirmation. For destructive operations (`delete`, `purge`, `deallocate`), spell out what will be removed and from which resource group/region.

## Step 4: Execute and report

- Run read commands directly and report the output clearly.
- Default to `--output table` for human-readable results; use `--output json` or `--query <jmespath>` when the user needs structured data or a specific field.
- For multi-step tasks, present the plan (subscription, resource group, sequence of commands) before executing.
- Include `--subscription <id-or-name>` on every command, even read-only ones, so the operation is unambiguous.
- For long-running operations, prefer `--no-wait` only when the user has asked for it; otherwise let the command block so errors surface.
- If a command fails, show the error and suggest remediation (often a missing extension — `az extension add --name <ext>` — or a permissions issue).

## Common patterns

```bash
# List subscriptions the user can access
az account list --output table

# List resource groups in a subscription
az group list --subscription <sub> --output table

# Show a specific resource
az resource show --subscription <sub> --ids <resource-id>

# Preview changes before deploying an ARM/Bicep template (read-only what-if)
az deployment group what-if \
  --subscription <sub> \
  --resource-group <rg> \
  --template-file main.bicep
```

## Rules

- Never run `az account set` to change the default subscription without explicit user permission. Use `--subscription` per-command instead.
- Never run an update command without confirmation, even if the user's request seems to imply consent — quote the exact command back and wait for "yes".
- Never log or echo secrets returned by commands like `az keyvault secret show` or `az ad sp create-for-rbac`. Mention that the value was retrieved without printing it, unless the user explicitly asks to see it.
