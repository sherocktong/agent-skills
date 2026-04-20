Respond to the user's request in markdown format and copy the response to the clipboard.

Steps:
1. Process the user's request and compose the response in well-formatted markdown
2. Copy the markdown response to the clipboard as UTF-8 using `pbcopy`: `printf '%s' '<markdown>' | pbcopy`
3. Confirm to the user that the response has been copied
