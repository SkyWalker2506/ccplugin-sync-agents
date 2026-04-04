# sync-agents — Claude Code Plugin

by [Musab Kara](https://linkedin.com/in/musab-kara-85580612a) · [GitHub](https://github.com/SkyWalker2506)

Validate and sync agent `.md` frontmatter ↔ `agent-registry.json`. For use with the Multi-Agent OS setup.

## Install

```bash
claude plugin install sync-agents@musabkara-claude-marketplace
```

## Commands

| Command | Description |
|---------|-------------|
| `/sync-agents` | Validate agents (--check) |
| `/sync-agents --fix` | Auto-fix mismatches |
| `/sync-agents --stats` | Show agent statistics |

## Requires

[SkyWalker2506/claude-config](https://github.com/SkyWalker2506/claude-config) installed with agents/ directory.

## Part of

- [claude-config](https://github.com/SkyWalker2506/claude-config) — Multi-Agent OS for Claude Code (110 agents, local-first routing)
- [Plugin Marketplace](https://github.com/SkyWalker2506/claude-marketplace) — Browse & install all 14 plugins
