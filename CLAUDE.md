## What This Vault Is For

This is a template for an **LLM Wiki** — a persistent, compounding knowledge base for Obsidian, driven by an AI agent (Claude Code or Cursor). Drop any source, ask any question, and the wiki grows richer with every session. Based on [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## Getting Started

This repo is a template. There are two ways to start:

1. **Use the checkout as your vault** — clone or check out this repo and open the folder directly in Obsidian (**Manage Vaults → Open folder as vault**). Point your agent (Claude Code or Cursor) at the same folder and start ingesting.
2. **Copy into an existing folder** — copy every folder and file from this repo (`.claude/`, `.cursor/`, `_templates/`, `wiki/`, `CLAUDE.md`, etc.) into the folder where your documents already live, then open that folder in Obsidian and point your agent at it.

Either way: drop a source into `.raw/`, then tell the agent `ingest [filename]`. Ask any question and the agent reads the index first, then drills into relevant pages.

## Vault Structure

```
.claude/        agent config — skills/, commands/, agents/, hooks/
.processed/     sources moved here after ingestion (gitignored)
.raw/           source documents — immutable, the agent reads but never modifies
_attachments/   images and PDFs referenced by wiki pages
_templates/     Obsidian Templater templates (concept, entity, source, question, comparison)
wiki/           agent-generated knowledge base
```

## How the Wiki Works

- **Read `wiki/hot.md` first** — recent session context (~500 words), loaded at session start.
- **`wiki/index.md`** — master catalog; read after the hot cache, then drill into individual pages.
- **`wiki/log.md`** — append-only operation log. Never edit past entries.
- **Use wikilinks** (`[[Note Name]]`) for all internal references in wiki pages.
- Run `lint the wiki` every 10–15 ingests to catch orphans and gaps.

## Skills & Commands

Skills live under `.claude/skills/<name>/SKILL.md`. The agent reads the matching skill when your request matches its trigger phrases.

| Skill / Command | Use it to |
|---|---|
| `/wiki` | Set up, scaffold, or route to a sub-skill |
| `ingest [source]` | Ingest one or many sources (creates 8–15 wiki pages) |
| `query: [question]` / `what do you know about X?` | Answer from wiki content with citations |
| `lint the wiki` | Health check: orphans, dead links, gaps |
| `/save` | File the current conversation as a structured wiki note |
| `/autoresearch [topic]` | Autonomous research loop: search, fetch, synthesize, file |
| `/canvas` | Visual layer: add images, PDFs, and notes to an Obsidian canvas |

Supporting skills: `defuddle` (clean web pages before ingest), `wiki-fold` (roll up log entries into meta-pages), and `obsidian-markdown` / `obsidian-bases` (Obsidian syntax references).

## Naming Conventions

- Wiki pages use **Title Case with spaces** (e.g. `Product Development Framework.md`)
- Source summary pages use **kebab-case** to match their `.raw/` filenames (e.g. `beyond-vibe-checks.md`)
- GitHub repo entity pages use **kebab-case** to match repository names

Sources and repo entities preserve their original identifiers. This is intentional.

## Cross-Project Access

To reference this wiki from another project, add to that project's CLAUDE.md:

```markdown
## Wiki Knowledge Base
Path: /path/to/this/vault

When you need context not already in this project:
1. Read wiki/hot.md first (recent context, ~500 words)
2. If not enough, read wiki/index.md
3. If you need domain specifics, read wiki/<domain>/_index.md
4. Only then read individual wiki pages

Do NOT read the wiki for general coding questions or things already in this project.
```

## MCP (Optional)

If you configure the MCP server, the agent can read and write vault notes directly.
See `.claude/skills/wiki/references/mcp-setup.md` for setup instructions.
