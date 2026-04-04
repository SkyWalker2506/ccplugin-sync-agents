---
description: Validate and sync agent .md frontmatter with agent-registry.json
argument-hint: [--check|--fix|--stats]
allowed-tools: [Bash, Read, Glob]
---

# Agent Sync

Validate that agent `.md` files and `agent-registry.json` are in sync.

## Instructions

Run based on `$ARGUMENTS`:
- `--check` (default): `python3 ~/Projects/claude-config/config/sync_agents.py --check`
- `--fix`: `python3 ~/Projects/claude-config/config/sync_agents.py --fix`
- `--stats`: `python3 ~/Projects/claude-config/config/sync_agents.py --stats`
- no args: run `--check`

Report results. If `--fix` is used, show what was changed.
If sync_agents.py is not found, tell user to run `./install.sh`.
