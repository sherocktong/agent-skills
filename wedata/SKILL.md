# WeData Skill

Use this skill when the user asks about Tencent Cloud WeData CLI, WeData Bundle, CI/CD, or data development workflows using the `wedata` command-line tool.

## Overview

Tencent Cloud WeData provides a CLI (`wedata`) and a Bundle system for CI/CD-driven data development. The Bundle describes WeData resources (workflows, tasks, resource files, project parameters, alarm rules, events) as source files, enabling version control and automated deployment pipelines (e.g., GitLab CI, GitHub Actions).

## Installation & Management

Install the WeData client using the official install script.

**macOS / Linux:**
```bash
curl https://wedata-agent-sg-1257305158.cos.ap-singapore.myqcloud.com/bundle/install-script/linux/install.sh | sudo sh
```

**Windows:**
```bash
curl.exe -fsSL https://wedata-agent-sg-1257305158.cos.ap-singapore.myqcloud.com/bundle/install-script/windows/install.bat -o install.bat && install.bat
```

**Other commands:**
- Uninstall: `sudo sh install.sh --uninstall`
- Upgrade: `sudo sh install.sh --upgrade`
- Get version: `wedata -v` or `wedata --version`

## Authentication

### Redirect Login (Interactive)
```bash
wedata auth login --host '${workspace-url}'
```
Or simply:
```bash
wedata auth login
```
Follow the browser prompts. First-time login requires CAM authorization. Session lasts ~12 hours.

### Fixed Key Login (For CI/CD or EntraID)
1. Log in via the web UI (EntraID if needed).
2. Run `wedata auth login` once to write temporary credentials to `~/.wedata/wedata.json`.
3. Edit `~/.wedata/wedata.json` and set `ExpireTime` to `0` for permanent CLI usage.
4. Fill in `Token`, `SecretId`, `SecretKey` from Tencent Cloud Identity Center > Access Credentials.

**Describe current auth config:**
```bash
wedata auth describe                    # default profile
wedata auth describe -p ${profile_name}
wedata auth describe --sensitive       # show secrets
```

## Bundle Package Structure

A bundle is defined by `wedata.yml` and resource YAML files.

### wedata.yml
```yaml
bundle:
  name: myBundle
  uuid: <uuid>
  wedata_cli_version: '0.18.*'

include:
  - resources/*/*.yml

sync:
  include:
    - "resources/**"
    - "src/**"
  exclude:
    - "tests/README.md"

targets:
  dev:
    mode: development
    default: true
    workspace:
      host: https://<region>.wedata.cloud.tencent.com/studio/datadev/workflow?ProjectId=<id>
  prod:
    mode: production
    workspace:
      host: https://<region>.wedata.cloud.tencent.com/studio/datadev/workflow?ProjectId=<id>
      root_path: /Workspace/Users/<user>/.bundle/${bundle.name}/${bundle.target}

variables:
  my_var:
    description: "example"
    default: "value"
    type: string
```

- `mode: development` → dev environment, resources prefixed with `[dev username]`, schedules paused.
- `mode: production` → prod environment, modifies official tasks directly.
- `variables` can be overridden per target under `targets.<name>.variables`.
- Use `${var.<name>}` in any resource YAML to reference variables.

### Resource Types

Resources are defined under `resources/` as YAML files.

| Resource Type | Key in YAML | Notes |
|---|---|---|
| Workflow (Task Scheduling) | `workflows` | Cycle scheduling configured at workflow level |
| Task (Task Scheduling) | `tasks` | Python, SQL, Shell, etc. Code in `src/task/` |
| Workflow (Workflow Scheduling) | `triggerWorkflows` | Scheduling configured at workflow level |
| Task (Workflow Scheduling) | `triggerTasks` | Under a `triggerWorkflow` |
| Resource File | `resourceFiles` | JAR, Python libs, etc. Upload to COS |
| Project Parameter | `projectParams` | Key-value parameters |
| Alarm Rule | `alarms` | Failure, timeout, success alarms |
| Event | `events` | MIN, HOUR, DAY types |

## Core Bundle Commands

All commands support `-t <target>` (dev/prod) and `-p <profile_name>`.

### Initialize
```bash
wedata bundle init -p ${profile_name}
```
Select scheduling mode (`workflow_trigger` or `task_trigger`) and resource types to scaffold.

### Generate (Pull Remote Resources)
```bash
wedata bundle generate task          --existing-task-id          <id> -t dev -p ${profile_name}
wedata bundle generate workflow      --existing-workflow-id      <id> -t dev -p ${profile_name}
wedata bundle generate event         --existing-event-id         <id> -t dev -p ${profile_name}
wedata bundle generate alarm         --existing-alarm-id         <id> -t dev -p ${profile_name}
wedata bundle generate resourceFile  --existing-resourceFile-id  <id> -t dev -p ${profile_name}
wedata bundle generate projectParam  --existing-projectParam-name <name> -t dev -p ${profile_name}
```

For workflow scheduling mode, use `triggerTask` and `triggerWorkflow` instead of `task` and `workflow`.

### Bind / Unbind
Bind links local resources to remote ones so local changes sync on deploy.

```bash
# Bind
wedata bundle deployment bind task          ${resource_key} ${task_id}          -t dev -p ${profile_name}
wedata bundle deployment bind workflow      ${resource_key} ${workflow_id}      -t dev -p ${profile_name}
wedata bundle deployment bind event         ${resource_key} ${event_id}         -t dev -p ${profile_name}
wedata bundle deployment bind alarm         ${resource_key} ${alarm_id}         -t dev -p ${profile_name}
wedata bundle deployment bind resourceFile  ${resource_key} ${resourceFile_id}  -t dev -p ${profile_name}
wedata bundle deployment bind projectParam  ${resource_key} ${projectParam_name} -t dev -p ${profile_name}

# Unbind
wedata bundle deployment unbind task     ${resource_key} -t dev -p ${profile_name}
wedata bundle deployment unbind workflow ${resource_key} -t dev -p ${profile_name}
# ... same pattern for other resource types
```

For workflow scheduling mode, use `triggerTask` and `triggerWorkflow`.

### Run
```bash
wedata bundle run task     ${resource_key} -t dev -p ${profile_name} --params "key1=val1&key2=val2"
wedata bundle run workflow ${resource_key} -t dev -p ${profile_name} --params "key1=val1&key2=val2"
```

For workflow scheduling mode, use `triggerTask` and `triggerWorkflow`.

### Validate
```bash
wedata bundle validate -t dev -p ${profile_name}
```

### Deploy
```bash
wedata bundle deploy -t dev -p ${profile_name} -s true
```
`-s true` submits changes to scheduling. Without it, only a saved version is created in the orchestration space.

### Destroy
```bash
wedata bundle destroy -t dev -p ${profile_name}
```
Deletes bound remote tasks and removes bundle resources from remote storage.

### Summary & Help
```bash
wedata bundle summary
wedata bundle -help
```

## CI/CD Integration (GitLab Example)

Typical `.gitlab-ci.yml` stages:

```yaml
stages:
  - build

build_dev:
  stage: build
  only:
    - dev
  script:
    - curl https://wedata-agent-sg-1257305158.cos.ap-singapore.myqcloud.com/bundle/install-script/linux/install.sh | sudo sh
    - wedata bundle validate -t dev
    - wedata bundle deploy -t dev

build_prod:
  stage: build
  only:
    - merge_requests
    - prod
  script:
    - curl https://wedata-agent-sg-1257305158.cos.ap-singapore.myqcloud.com/bundle/install-script/linux/install.sh | sudo sh
    - wedata bundle validate -t prod
    - wedata bundle deploy -t prod
```

For non-interactive CI/CD, configure fixed-key auth via `~/.wedata/wedata.json` with `ExpireTime: 0`.

## Common FAQs

- **Task names get `dev_username` prefix after deploy?**
  You are deploying to a `dev` target. Use `prod` target to modify official tasks directly.

- **What is workflow scheduling vs task scheduling?**
  - Workflow scheduling: scheduling info at workflow level, simpler dependencies.
  - Task scheduling: scheduling info at task level, supports complex dependencies.

- **Where do I get taskId / workflowId?**
  - Task ID: Task properties panel in Studio.
  - Workflow ID: Workflow universal settings panel.
