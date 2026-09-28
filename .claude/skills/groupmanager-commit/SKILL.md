---
name: groupmanager-commit
description: Use automatically whenever preparing or creating a git commit in this repository (GroupManager, remote github.com/5nntnd/GroupManager). Ensures no Co-Authored-By trailer is ever added to commits here.
---

# GroupManager commits: no co-author trailer

This repo is 5nntnd's personal public project (`https://github.com/5nntnd/GroupManager`). Commits here must NOT carry a `Co-Authored-By` trailer, even though the default Claude Code commit workflow normally appends one.

## Rules

- Never add `Co-Authored-By: Claude ... <noreply@anthropic.com>` (or any other co-author trailer) to a commit made in this repository.
- Use the exact commit message the user provides, verbatim — no appended trailers, no extra footer lines.
- If the user hasn't given a commit message yet, ask for one rather than generating one — they write their own commit messages for this repo.
- Everything else about the normal commit workflow still applies: check `git status`/`git diff` before staging, stage specific files by name (avoid `git add -A`/`.`), and never use `--no-verify` or `--amend` unless explicitly asked.
- This only overrides the co-author trailer behavior — it does not authorize pushing. Push only when the user separately asks.
