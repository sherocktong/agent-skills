---
name: es
description: Use the es CLI to query and check logs from Elasticsearch
triggers:
  - "check elasticsearch"
  - "search logs"
  - "query elastic"
  - "es logs"
  - "elasticsearch query"
---

# es skill

Use the `es` CLI to interact with Elasticsearch. The default URL is `http://localhost:9200`.

## Rules

- **NEVER use `curl` to call Elasticsearch endpoints directly.** Always use the `es` CLI.
- Before running any query, verify the Elasticsearch instance is reachable by listing indices. If it fails, stop immediately and inform the user:
  > "Elasticsearch at http://localhost:9200 is not available. Please check that the service is running or provide a different URL."

## Availability check

Run this first to confirm connectivity:

```bash
es indices
```

If the command errors, stop and notify the user before proceeding.

## Commands

### List indices

```bash
es indices
```

### Search logs

```bash
es search <index> [size] [sort] [sort_field] [time_range] [query]
```

Parameters (all positional):
| Parameter    | Default      | Description |
|-------------|--------------|-------------|
| `index`     | required     | Index name or pattern, e.g. `logs-*` |
| `size`      | `20`         | Number of results |
| `sort`      | `desc`       | `asc` or `desc` |
| `sort_field`| `@timestamp` | Field for sorting and time filtering |
| `time_range`| (none)       | e.g. `5m`, `1h`, `7d`, or `2026-03-08T10:00:00..2026-03-08T11:00:00` |
| `query`     | (none)       | Elasticsearch query DSL as JSON |

### Examples

```bash
# Latest 20 logs from an index
es search logs

# Last 5 minutes, 50 results
es search logs 50 desc @timestamp 5m

# Filter by level=error, last 1 hour
es search logs 50 desc @timestamp 1h '{"match":{"level":"error"}}'

# Search for "timeout" in message, last 1 hour
es search logs 20 desc @timestamp 1h '{"match":{"message":"timeout"}}'

# Multiple filters (comma-separated JSON objects)
es search logs 50 desc @timestamp 1h '{"term":{"level.keyword":"error"}},{"match":{"message":"timeout"}}'

# Absolute time range
es search logs 20 desc @timestamp 2026-03-08T10:00:00..2026-03-08T11:00:00

# Pipe to jq to pretty-print log sources
es search logs 20 desc @timestamp 5m | jq '.hits.hits[]._source'
```

## Workflow

1. Confirm the Elasticsearch URL (default: `http://localhost:9200`). The `es` CLI uses this automatically; no `--url` flag is needed unless the tool supports it.
2. Run `es indices` to confirm connectivity. If unavailable, stop and notify the user.
3. Identify the relevant index from the indices list if the user has not specified one.
4. Run `es search` with appropriate parameters based on the user's request.
5. Pipe to `jq` when presenting results for readability.
6. Summarize counts, highlight errors or anomalies, and quote relevant log lines.
