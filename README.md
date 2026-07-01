# Obsidian LLM Wiki Template

A template Obsidian vault that turns your AI agent (Claude Code or Cursor) into a running notetaker — it builds and maintains a persistent, compounding wiki. Every source you add gets integrated. Every question you ask pulls from everything that has been read. Knowledge compounds like interest.

Based on [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). Skills, commands, agents, and hooks are bundled for both Claude Code and Cursor.

---

## What It Does

You drop sources. The agent reads them, extracts entities and concepts, updates cross-references, and files everything into a structured Obsidian vault. The wiki gets richer with every ingest.

You ask questions. The agent reads the hot cache (recent context), scans the index, drills into relevant pages, and synthesizes an answer. It cites specific wiki pages, not training data.

You lint. The agent finds orphans, dead links, stale claims, and missing cross-references, so your wiki stays healthy without manual cleanup.

At the end of every session, the agent updates a hot cache. The next session starts with full recent context, no recap needed.

---

## Getting Started

This repo is a template. Pick whichever path fits:

### Option 1: Use the checkout as your vault

Clone or check out this repo and open the folder directly in Obsidian:
**Manage Vaults → Open folder as vault → select this folder.**

Then open the same folder in Claude Code or Cursor and start ingesting.

### Option 2: Copy into an existing folder

Copy every folder and file from this repo into the folder where your documents already live:

```
.claude/        skills, commands, agents, hooks
_templates/     Obsidian Templater templates
wiki/           starter knowledge base
CLAUDE.md       agent instructions (auto-loaded)
.gitignore
.obsidian/      pre-configured graph view, CSS snippets, plugin list
```

Open that folder in Obsidian and point your agent at it.

### Then

Drop a source into `.raw/`, and tell the agent:

```
ingest [filename]
```

Ask any question (`what do you know about X?`) and the agent reads the index first, then drills into relevant pages. Type `/wiki` to scaffold or check setup status.

---

## Commands

| You say | The agent does |
|---------|------------|
| `/wiki` | Setup check, scaffold, or continue where you left off |
| `ingest [file]` | Read source, create 8–15 wiki pages, update index and log |
| `ingest all of these` | Batch process multiple sources, then cross-reference |
| `what do you know about X?` | Read index → relevant pages → synthesize answer |
| `/save` | File the current conversation as a wiki note |
| `/save [name]` | Save with a specific title (skips the naming question) |
| `/autoresearch [topic]` | Run the autonomous research loop: search, fetch, synthesize, file |
| `/canvas` | Open or create the visual canvas, list zones and nodes |
| `/canvas add image [path]` | Add an image (URL or local path) to the canvas with auto-layout |
| `/canvas add text [content]` | Add a markdown text card to the canvas |
| `/canvas add pdf [path]` | Add a PDF document as a rendered preview node |
| `/canvas add note [page]` | Pin a wiki page as a linked card on the canvas |
| `lint the wiki` | Health check: orphans, dead links, gaps, suggestions |
| `update hot cache` | Refresh hot.md with the latest context summary |

---

## Wiki Modes

The `/wiki` scaffold supports several vault shapes. Modes can be combined.

| Mode | Use when |
|------|---------|
| A: Website | Sitemap, content audit, SEO wiki |
| B: GitHub | Codebase map, architecture wiki |
| C: Business | Project wiki, competitive intelligence |
| D: Personal | Second brain, goals, journal synthesis |
| E: Research | Papers, concepts, thesis |
| F: Book/Course | Chapter tracker, course notes |

---

## What Gets Created

A typical scaffold creates:

- Folder structure for your chosen mode
- `wiki/index.md`: master catalog
- `wiki/log.md`: append-only operation log
- `wiki/hot.md`: recent context cache
- `wiki/overview.md`: executive summary
- `wiki/meta/dashboard.base`: Bases dashboard (primary, native Obsidian)
- `_templates/`: Obsidian Templater templates for each note type
- `.obsidian/snippets/vault-colors.css`: color-coded file explorer
- `CLAUDE.md`: auto-loaded agent instructions

---

## File Structure

```
obsidian-llm-wiki-template/
├── .claude/
│   ├── skills/                  # 11 skills (wiki, wiki-ingest, wiki-query, wiki-lint,
│   │                            #   save, autoresearch, canvas, defuddle, wiki-fold,
│   │                            #   obsidian-markdown, obsidian-bases)
│   ├── commands/                # /wiki, /save, /autoresearch, /canvas
│   ├── agents/                  # wiki-ingest + wiki-lint subagents
│   └── hooks/                   # hot-cache + auto-commit hooks (hooks.json)
├── _templates/                  # concept, entity, source, question, comparison
├── wiki/
│   ├── index.md                 # master catalog
│   ├── overview.md              # executive summary
│   ├── hot.md                   # recent context cache
│   ├── log.md                   # append-only operation log
│   ├── getting-started.md       # onboarding walkthrough (inside the vault)
│   ├── concepts/                # concept pages
│   ├── entities/                # entity pages
│   ├── domains/                 # domain sub-indexes
│   ├── sources/                 # populated by your first ingest
│   └── meta/                    # dashboards and meta pages
├── _attachments/                # images and PDFs referenced by wiki pages (gitignored)
├── .raw/                        # source documents — immutable (gitignored)
├── .processed/                  # sources moved here after ingestion (gitignored)
├── .obsidian/                   # pre-configured graph view, snippets, plugin list
├── CLAUDE.md                    # agent instructions
└── README.md                    # this file
```

---

## MCP Setup (Optional)

MCP lets the agent read and write vault notes directly without copy-paste.

Option A (REST API based):
1. Install the Local REST API plugin in Obsidian
2. Copy your API key
3. Run:
```bash
claude mcp add-json obsidian-vault '{
  "type": "stdio",
  "command": "uvx",
  "args": ["mcp-obsidian"],
  "env": {
    "OBSIDIAN_API_KEY": "your-key",
    "OBSIDIAN_HOST": "127.0.0.1",
    "OBSIDIAN_PORT": "27124",
    "NODE_TLS_REJECT_UNAUTHORIZED": "0"
  }
}' --scope user
```

Option B (filesystem based, no plugin needed):
```bash
claude mcp add-json obsidian-vault '{
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@bitbonsai/mcpvault@latest", "/path/to/your/vault"]
}' --scope user
```

See `.claude/skills/wiki/references/mcp-setup.md` for details.

---

## Obsidian Plugins

### Core plugins (built into Obsidian, no install needed)

| Plugin | Purpose |
|--------|---------|
| **Bases** | Powers `wiki/meta/dashboard.base` (native database views). Available since Obsidian v1.9.10. |
| **Properties** | Visual frontmatter editor |
| **Backlinks**, **Outline**, **Graph view** | Standard navigation |

### Community plugins

The shipped `.obsidian/community-plugins.json` already lists these as enabled, but the plugin code is not tracked in git. Install each from **Settings → Community Plugins → Browse**:

| Plugin | Purpose |
|--------|---------|
| **Templater** | Auto-fills frontmatter from `_templates/` |
| **Dataview** | Optional/legacy — only needed for the legacy `wiki/meta/dashboard.md` queries |
| **Calendar** | Right-sidebar calendar with word count + task dots |
| **Thino** | Quick memo capture panel |
| **Excalidraw** | Freehand drawing canvas, annotate images |
| **Banners** | Notion-style header image via `banner:` frontmatter |

Consider also installing **Obsidian Git** (auto-commits the vault) and the
**[Obsidian Web Clipper](https://obsidian.md/clipper)** browser extension (sends web pages to `.raw/`).

---

## CSS Snippets

Three snippets ship in `.obsidian/snippets/` and are already enabled in `appearance.json`:

| Snippet | Effect |
|---------|--------|
| `vault-colors` | Color-codes `wiki/` folders by type in the file explorer |
| `ITS-Dataview-Cards` | Turns Dataview `TABLE` queries into visual card grids |
| `ITS-Image-Adjustments` | Fine-grained image sizing: append `\|100` to any image embed |

---

## Banner

Add to any wiki page frontmatter (requires the Banners plugin):

```yaml
banner: "_attachments/images/your-image.png"
banner_icon: "🧠"
```

The page renders a full-width header image in Obsidian. Great for hub pages and overviews.

---

## AutoResearch: program.md

The `/autoresearch` command is configurable. Edit `.claude/skills/autoresearch/references/program.md` to control:

- What sources to prefer (academic, official docs, news)
- Confidence scoring rules
- Max rounds and max pages per session
- Domain-specific constraints

The default program works for general research. A medical researcher would add "prefer PubMed"; a business analyst would add "focus on market data and filings".

---

## Cross-Project Access

Point any other project at this vault by adding to that project's `CLAUDE.md`:

```markdown
## Wiki Knowledge Base
Path: /path/to/this/vault

When you need context not already in this project:
1. Read wiki/hot.md first (recent context cache)
2. If not enough, read wiki/index.md
3. If you need domain details, read the relevant domain sub-index
4. Only then drill into specific wiki pages

Do NOT read the wiki for general coding questions or tasks unrelated to [domain].
```

---

*Based on [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).*
