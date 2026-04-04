---
name: sync-agents
description: This skill activates when the user mentions "sync agents", "agent registry", "validate agents", "agent mismatch", "update registry", or when adding/removing agent .md files from the agents/ directory. Also runs automatically during daily-check.
version: 1.0.0
---

# Agent Sync Skill

Keeps `agents/*.md` frontmatter in sync with `config/agent-registry.json`.

## What It Validates

- Every `.md` in `agents/**/*.md` has required frontmatter fields (`id`, `name`, `category`, `primary_model`, `status`)
- Every `id` in agent `.md` files exists in `agent-registry.json`
- Every agent in `agent-registry.json` has a corresponding `.md` file
- No duplicate IDs
- Valid model names (sonnet, opus, haiku, or known free/local models)
- Status values are `active` or `pool`

## Usage

```bash
python3 config/sync_agents.py --check   # validate only
python3 config/sync_agents.py --fix     # auto-fix mismatches
python3 config/sync_agents.py --stats   # show statistics
```

## Auto-Fix Behavior

`--fix` will:
1. Add missing agents to registry (from .md frontmatter)
2. Remove orphaned registry entries (no .md file)
3. Update registry fields from .md frontmatter (one-way: .md → registry)

## Integration

Runs automatically in:
- `daily-check.sh` (`--check` mode)
- `install.sh` Phase 10 (`--check` with warning on failure)
- Pre-commit hook (if enabled)
