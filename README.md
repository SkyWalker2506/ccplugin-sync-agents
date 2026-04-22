# sync-agents — Claude Code Plugin

by [Musab Kara](https://linkedin.com/in/musab-kara-85580612a) · [GitHub](https://github.com/SkyWalker2506)

Validate and sync agent `.md` frontmatter ↔ `agent-registry.json`. For use with the Multi-Agent OS setup.

## Install

```bash
claude plugin install sync-agents@musabkara-claude-marketplace
```

## Quick Start

```bash
# 1. Check — see what's out of sync
/sync-agents

# 2. Preview what --fix would change (dry-run, no writes)
/sync-agents --diff

# 3. Apply fixes
/sync-agents --fix

# 4. Show statistics
/sync-agents --stats
```

## Commands

| Command | Description |
|---------|-------------|
| `/sync-agents` | Validate agents (--check mode, default) |
| `/sync-agents --diff` | Preview what --fix would change (dry-run) |
| `/sync-agents --fix` | Auto-fix mismatches (adds missing, removes orphaned) |
| `/sync-agents --stats` | Show agent statistics (count, active/pool, by category, by model) |
| `/sync-agents --check --json` | Machine-readable JSON output for CI/automation |
| `/sync-agents --path DIR` | Use alternate agents directory |
| `/sync-agents --quiet` | Errors only, suppress info messages |

## What It Validates

- Every `agents/**/*.md` has required frontmatter fields (`id`, `name`, `category`, `primary_model`, `status`, `allowed-tools`)
- Every `id` in agent `.md` files exists in `agent-registry.json`
- Every agent in `agent-registry.json` has a corresponding `.md` file
- No duplicate IDs
- Valid model names (`sonnet`, `opus`, `haiku`, or known free/local models)
- Status values are `active` or `pool`

## Auto-Fix Behavior (`--fix`)

Always run `--diff` first to preview changes.

1. Adds missing agents to registry (from `.md` frontmatter)
2. Removes orphaned registry entries (no `.md` file)
3. Updates registry fields from `.md` frontmatter (one-way: `.md` → registry)

## CI Usage

```bash
# In GitHub Actions or daily-check.sh:
/sync-agents --check --json --quiet
# exit 0 = clean, exit 1 = mismatches, exit 2 = error
```

## Auto-Runs In

- `daily-check.sh` — `--check` mode
- `install.sh` Phase 10 — `--check` with warning on failure

## Requires

[SkyWalker2506/claude-config](https://github.com/SkyWalker2506/claude-config) installed with `agents/` directory and `config/sync_agents.py`.

Path is auto-detected. Checks in order:
1. `~/Projects/claude-config/config/sync_agents.py`
2. `~/.claude-config/config/sync_agents.py`
3. `./config/sync_agents.py`

## Part of

- [claude-config](https://github.com/SkyWalker2506/claude-config) — Multi-Agent OS for Claude Code (134 agents, local-first routing)
- [Plugin Marketplace](https://github.com/SkyWalker2506/claude-marketplace) — Browse & install all 18 plugins
- [ClaudeHQ](https://github.com/SkyWalker2506/ClaudeHQ) — Claude ecosystem HQ
