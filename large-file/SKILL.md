Prevent context overflow when working with large files by reading them incrementally and surgically.

## Threshold

A file is "large" if it exceeds **500 lines or ~50 KB**. Apply this protocol whenever you are about to read such a file.

## Protocol

### Step 1 — Check size before reading
Before calling Read on any file, run:
```
wc -l <file>  # line count
wc -c <file>  # byte count
```
If the file exceeds the threshold, do NOT read it whole. Proceed to Step 2.

### Step 2 — Get a structural overview
Use `smart_outline` (if available) to see all symbols, classes, and functions with signatures but no bodies:
```
mcp__plugin_claude-mem_mcp-search__smart_outline(file_path="<path>")
```
This gives you the map without paying the token cost of the bodies.

### Step 3 — Find relevant sections first
Use Grep to locate the specific lines you need before reading:
```
Grep(pattern="<symbol or keyword>", path="<file>", output_mode="content", -n=true)
```
Note the line numbers, then read only the relevant range.

### Step 4 — Read in targeted chunks
Use the `offset` and `limit` parameters on Read to fetch only what you need:
```
Read(file_path="<path>", offset=<start_line - 1>, limit=<number_of_lines>)
```
Keep each chunk under ~200 lines. Iterate if you need more context.

### Step 5 — Use smart_unfold for specific symbols
If you identified a specific function or class in Step 2, unfold just that symbol:
```
mcp__plugin_claude-mem_mcp-search__smart_unfold(file_path="<path>", symbol_name="<name>")
```

## Quick-reference decision tree

```
Need to read a file?
├── < 500 lines → Read normally
└── ≥ 500 lines
    ├── Need structure overview? → smart_outline
    ├── Need a specific symbol? → Grep for line number → Read(offset, limit)  OR  smart_unfold
    ├── Need to find all usages? → Grep across the repo
    └── Need to understand a section? → Read(offset=<section_start>, limit=150)
```

## Rules
- Never call Read without offset/limit on files ≥ 500 lines.
- Prefer Grep → targeted Read over reading entire files.
- If you must read the whole file, read in 200-line chunks and stop once you have the answer.
- Never load multiple large files into context at the same time without first pruning earlier content.
