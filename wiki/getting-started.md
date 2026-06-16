---
type: meta
title: "Getting Started"
updated: 2026-05-20
created: 2026-04-07
tags:
  - meta
  - onboarding
status: evergreen
related:
  - "[[index]]"
  - "[[overview]]"
  - "[[CPRE Certification]]"
---

# Getting Started

Welcome. This vault is a compounding knowledge base for the **<>**, built with Claude and Obsidian.

---

## Where to Start

| Page | Purpose |
|------|---------|
| [[index]] | Master catalogue of every page, grouped by type |
| [[overview]] | Executive summary of vault scope, EUs, key principles |
| [[hot]] | Recent session context cache (~500 words) |
| [[log]] | Operation log (newest at top) |

For exam preparation, start with [[overview]], then walk through the FL pages in order: FL01 Introduction and Overview of RE → FL07 Tool Support.

---

## How the Vault Grows

Drop a new source into `.raw/`, then ask Claude:

```
ingest [filename]
```

Claude reads the source, creates or updates wiki pages under `wiki/`, cross-references everything, and appends to `wiki/log.md` and `wiki/hot.md`.

For questions answered from the vault:

```
what do you know about [topic]?
```

Claude reads the hot cache, scans the index, drills into relevant pages, and synthesises an answer citing specific wiki pages — not training data.

---

## Hot Cache

`wiki/hot.md` is a short summary of recent vault context. It loads at the start of every session so Claude knows where work was left off. Refresh it manually any time with: `update hot cache`.

---

## Key Commands

| You say | Claude does |
|---------|-------------|
| `ingest [file]` | Creates wiki pages from a source in `.raw/` |
| `what do you know about X?` | Queries the wiki, cites pages |
| `/save` | Files this conversation as a wiki note |
| `/autoresearch [topic]` | Searches the web, ingests results autonomously |
| `lint the wiki` | Health check — finds orphans, gaps, stale links |
| `update hot cache` | Refreshes the session context summary |

---

*Built on the [LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) by Andrej Karpathy.*
