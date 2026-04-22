---
name: plain-text
description: Respond to the user's request in plain text and copy the response to the clipboard.
---

Respond to the user's request in plain text and copy the response to the clipboard.

Steps:
1. Process the user's request and compose the response as plain text (no markdown formatting)
2. Copy the plain text response to the clipboard as UTF-8 using `pbcopy`: `printf '%s' '<text>' | pbcopy`
3. Confirm to the user that the response has been copied
