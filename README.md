# sync-agents — Claude Code Plugin

by [Musab Kara](https://linkedin.com/in/musab-kara-85580612a) · [GitHub](https://github.com/SkyWalker2506)

Validate and sync agent `.md` frontmatter ↔ `agent-registry.json`. For use with the Multi-Agent OS setup.

## Install

```bash
claude plugin install SkyWalker2506/ccplugin-sync-agents
```

## Commands

| Command | Description |
|---------|-------------|
| `/sync-agents` | Validate agents (--check mode, default) |
| `/sync-agents --fix` | Auto-fix mismatches (adds missing, removes orphaned) |
| `/sync-agents --stats` | Show agent statistics |

## What It Validates

- Every `agents/**/*.md` has required frontmatter fields (`id`, `name`, `category`, `primary_model`, `status`)
- Every `id` in agent `.md` files exists in `agent-registry.json`
- Every agent in `agent-registry.json` has a corresponding `.md` file
- No duplicate IDs
- Valid model names (`sonnet`, `opus`, `haiku`, or known free/local models)
- Status values are `active` or `pool`

## Auto-Fix Behavior (`--fix`)

1. Adds missing agents to registry (from `.md` frontmatter)
2. Removes orphaned registry entries (no `.md` file)
3. Updates registry fields from `.md` frontmatter (one-way: `.md` → registry)

## Auto-Runs In

- `daily-check.sh` — `--check` mode
- `install.sh` Phase 10 — `--check` with warning on failure
- Pre-commit hook (if enabled)

## Requires

[SkyWalker2506/claude-config](https://github.com/SkyWalker2506/claude-config) installed with `agents/` directory and `config/sync_agents.py`.

## Part of

- [claude-config](https://github.com/SkyWalker2506/claude-config) — Multi-Agent OS for Claude Code (110 agents, local-first routing)
- [Plugin Marketplace](https://github.com/SkyWalker2506/claude-plugins) — Browse & install all plugins
