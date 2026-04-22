---
name: file
description: List all files mentioned in the current session, let the user pick one, and copy its path to the clipboard.
---

List all files mentioned in the current session, let the user pick one, and copy its path to the clipboard.

## Steps

1. **Parse the session transcript** to extract all file paths mentioned.

   Read the transcript JSONL from the path in the `CLAUDE_TRANSCRIPT` environment variable. Parse each line as JSON and collect file paths from:
   - Tool call `input` objects: fields named `file_path`, `path`, `notebook_path`, `file`, `src`, `dest`, `source`, `destination`, `from`, `to`
   - Tool result content (text that looks like an absolute or relative file path)
   - User and assistant message text (any tokens that look like file paths: start with `/`, `~/`, `./`, or contain a file extension like `.py`, `.ts`, `.md`, `.json`, `.yaml`, `.sh`, `.txt`, `.js`, `.tsx`, `.jsx`, `.go`, `.rs`, `.rb`, `.java`, `.c`, `.cpp`, `.h`, `.css`, `.html`, `.xml`, `.toml`, `.lock`, `.env`)

   Use this Bash one-liner to do the extraction (run with Bash tool):
   ```bash
   python3 - <<'EOF'
   import json, os, re, sys

   transcript_path = os.environ.get("CLAUDE_TRANSCRIPT", "")
   cwd = os.getcwd()
   paths = set()

   path_re = re.compile(
       r'(?:^|[\s"\',\[\(])(/(?:[^\s"\',\]\)]+))'  # absolute paths
       r'|(?:^|[\s"\',\[\(])(~[^\s"\',\]\)]+)'      # tilde paths
       r'|(?:^|[\s"\',\[\(])(\.{1,2}/[^\s"\',\]\)]+)' # relative paths
   )

   key_fields = {"file_path","path","notebook_path","file","src","dest","source","destination","from","to","output_file","input_file","filename"}

   def extract_from_value(v):
       if isinstance(v, str):
           for m in path_re.finditer(v):
               p = next(g for g in m.groups() if g)
               p = p.rstrip(".,;:)'\"")
               if p and not p.endswith(("/", ":")):
                   paths.add(p)
       elif isinstance(v, dict):
           for k, sub in v.items():
               if k in key_fields and isinstance(sub, str) and sub:
                   paths.add(sub)
               else:
                   extract_from_value(sub)
       elif isinstance(v, list):
           for item in v:
               extract_from_value(item)

   if transcript_path and os.path.exists(transcript_path):
       with open(transcript_path) as f:
           for line in f:
               line = line.strip()
               if not line:
                   continue
               try:
                   obj = json.loads(line)
               except json.JSONDecodeError:
                   continue
               extract_from_value(obj)

   # Normalize and filter
   result = []
   seen = set()
   for p in sorted(paths):
       expanded = os.path.expanduser(p)
       if not os.path.exists(expanded):
           continue
       if os.path.isdir(expanded):
           continue
       # Make relative if under cwd
       try:
           rel = os.path.relpath(expanded, cwd)
           display = rel if not rel.startswith("..") else expanded
       except ValueError:
           display = expanded
       if display not in seen:
           seen.add(display)
           result.append(display)

   for r in sorted(result):
       print(r)
   EOF
   ```

2. **If no files are found**, tell the user: "No files were found in the current session."

3. **Present the list** to the user as a numbered list and ask: "Which file path would you like to copy to the clipboard? Enter a number."

4. **Wait for the user's response** (a number). Validate the input is in range.

5. **Copy the selected path** to the clipboard:
   ```bash
   echo -n "<selected_path>" | pbcopy
   ```

6. Confirm to the user: "Copied `<selected_path>` to clipboard."
