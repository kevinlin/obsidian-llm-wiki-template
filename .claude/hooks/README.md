# Hooks

Agent hooks for the LLM Wiki vault, used by Claude Code. All hooks are defined in `hooks.json` in this folder.

## What They Do

| Event | Type | Purpose |
|---|---|---|
| `SessionStart` | command + prompt | Loads `wiki/hot.md` into context at the start of a session. The command `[ -f wiki/hot.md ] && cat wiki/hot.md` is the canonical check (safe in non-vault folders); the prompt complements it with semantic context restoration. Matcher: `startup` / `resume`. |
| `PostCompact` | prompt | Re-loads `wiki/hot.md` after context compaction. Hook-injected context does not survive compaction (only `CLAUDE.md` does), so this restores the hot cache mid-session. |
| `PostToolUse` | command | Auto-commits `wiki/` and `.raw/` changes after a Write or Edit. Guarded by `[ -d .git ]` (never errors outside a git repo) and `git diff --cached --quiet` (never creates empty commits). |
| `Stop` | prompt | At the end of a response, if wiki pages changed, asks the agent to refresh `wiki/hot.md` with a brief summary. |

## Getting Started

These hooks run automatically once the vault folder is opened with Claude Code — no setup needed. Every hook is guarded so it stays silent and exits cleanly in non-vault or non-git folders, which makes the config safe to keep in place everywhere.

To verify the hooks fire: open a Claude Code session in a folder with a populated `wiki/hot.md` and ask "what's in the hot cache?". If the agent already has that recent context, the hooks are working.

If they don't fire, copy the hook config from `hooks.json` into your user-level `~/.claude/settings.json`.
