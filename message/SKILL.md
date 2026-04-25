---
name: message
description: Respond to the user's request in plain text with support for emojis and lists, and copy the response to the clipboard. Use minimal spacing - only add a blank line when transitioning to a new section.
---

Respond to the user's request in plain text with support for emojis and lists, and copy the response to the clipboard. Use minimal spacing - only add a blank line when transitioning to a new section.

Steps:
- Process the user's request and compose the response as plain text
- Use emojis and lists where appropriate to enhance clarity and expression
- Use minimal spacing - only add a blank line when transitioning to a new section
- Copy the response to the clipboard as UTF-8 using `pbcopy`: `printf '%s' '<text>' | pbcopy`
- Confirm to the user that the response has been copied
