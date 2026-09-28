---
name: groupmanager-crlf
description: Use automatically whenever creating or editing text files in this repository (GroupManager, remote github.com/5nntnd/GroupManager). Write and save files with CRLF line endings, not LF.
---

# GroupManager files: use CRLF line endings

This repo is used on Windows, and its files should use CRLF (`\r\n`) line endings rather than LF (`\n`).

## Rules

- When creating a new file in this repository, write it with CRLF line endings.
- When editing an existing file, preserve CRLF line endings — don't let an edit introduce LF-only lines into an otherwise CRLF file.
- If a file already exists with LF endings and you're touching it substantially, normalize it to CRLF as part of the edit.
- This is a formatting convention only — it doesn't change what content to write, just the line-ending byte sequence.
