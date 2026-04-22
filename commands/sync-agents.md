---
description: Validate and sync agent .md frontmatter with agent-registry.json
argument-hint: [--check|--fix|--diff|--stats|--json|--path DIR|--quiet]
allowed-tools: [Bash, Read, Glob]
---

# Agent Sync

Validate that agent `.md` files and `agent-registry.json` are in sync.

## Path Detection

Before running, locate `sync_agents.py` by checking these paths in order:
1. `~/Projects/claude-config/config/sync_agents.py`
2. `~/.claude-config/config/sync_agents.py`
3. `./config/sync_agents.py`
4. Any path provided by `--path` flag

If not found in any location:
```
❌ sync_agents.py not found. Run ./install.sh to set up claude-config, or use --path to specify a custom location.
```

## Flags

| Flag | Behavior |
|------|----------|
| _(none)_ | Same as --check |
| `--check` | Validate only — show formatted summary, no changes |
| `--fix` | Auto-fix mismatches (adds missing, removes orphaned) |
| `--diff` | Show what --fix would change without applying (dry-run) |
| `--stats` | Show agent statistics breakdown |
| `--json` | Output results as JSON (combine with --check or --stats) |
| `--path DIR` | Use alternate agents directory instead of default |
| `--quiet` | Suppress info messages, show only errors and warnings |

## Instructions

### --check (default)

Run: `python3 {sync_agents_path} --check`

Format the output as:
```
Agent Sync — Check Mode
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Checked: N agents
  ✓ Passed: M
  ✗ Failed: K
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Issues:
  ✗ agent-name.md — missing field: allowed-tools
  ✗ agent-name2.md — id 'foo' not in registry
  ⚠️  registry entry 'bar-agent' has no .md file

Exit code: 0 (clean) | 1 (mismatches found) | 2 (script error)
```

### --fix

Run: `python3 {sync_agents_path} --fix`

Show what was changed:
```
Agent Sync — Fix Mode
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  + Added to registry: new-agent (from new-agent.md)
  - Removed from registry: orphaned-agent (no .md file)
  ~ Updated registry: agent-name.primary_model: sonnet → opus
Fixed: 3 changes applied
```

### --diff

Show what --fix would change, without applying:
```
Agent Sync — Diff Mode (dry-run)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Would add to registry:
  + new-agent (from new-agent.md)
Would remove from registry:
  - orphaned-agent (no .md file)
Would update in registry:
  ~ agent-name.primary_model: sonnet → opus
Total: 3 changes pending (run --fix to apply)
```

### --stats

Run: `python3 {sync_agents_path} --stats`

Format as:
```
Agent Statistics
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total agents:   134
  Active:        89 (66%)
  Pool:          45 (34%)

By category:
  code          23
  research      18
  data          15
  ...

By model:
  sonnet        98
  opus          22
  haiku         14

Validation score: 127/134 (95%) — 7 issues
```

### --json

Combine with --check or --stats to output JSON:
```json
{
  "mode": "check",
  "total": 134,
  "passed": 127,
  "failed": 7,
  "issues": [
    {
      "file": "agents/code/agent-name.md",
      "field": "allowed-tools",
      "message": "missing required field"
    }
  ],
  "exit_code": 1
}
```

### --path DIR

Override the default agents directory:
`python3 {sync_agents_path} --check --path /custom/agents/dir`

### --quiet

Suppress info-level output. Only show errors (✗) and warnings (⚠️). Useful for CI.

## Example Outputs

### Clean run (--check)
```
Agent Sync — Check Mode
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Checked: 134 agents
  ✓ Passed: 134
  ✗ Failed: 0

✅ All agents in sync. Exit code: 0
```

### Issues found (--check)
```
Agent Sync — Check Mode
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Checked: 134 agents
  ✓ Passed: 131
  ✗ Failed: 3

Issues:
  ✗ agents/code/refactor-agent.md — missing field: allowed-tools
  ✗ agents/research/market-analyst.md — id 'market-analyst' not in registry
  ⚠️  registry entry 'old-agent-v1' has no .md file

Run /sync-agents --diff to preview fixes, or --fix to apply.
Exit code: 1
```

## If sync_agents.py is not found

Tell the user:
```
❌ sync_agents.py not found. Run ./install.sh to set up claude-config.
   Checked: ~/Projects/claude-config/config/, ~/.claude-config/config/, ./config/
   Use --path to specify a custom location.
```
