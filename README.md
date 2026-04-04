# sync-agents — Claude Code Plugin

Validate and sync agent `.md` frontmatter ↔ `agent-registry.json`. For use with the Multi-Agent OS setup.

## Install

```bash
claude plugin install sync-agents@SkyWalker2506-claude-plugins
```

## Commands

| Command | Description |
|---------|-------------|
| `/sync-agents` | Validate agents (--check) |
| `/sync-agents --fix` | Auto-fix mismatches |
| `/sync-agents --stats` | Show agent statistics |

## Requires

[SkyWalker2506/claude-config](https://github.com/SkyWalker2506/claude-config) installed with agents/ directory.
