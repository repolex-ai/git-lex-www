---
title: Architecture
order: 3
nav_title: Architecture
---

## Design principle

git-lex treats git as the database. Instead of building a separate knowledge store and syncing it with git, git-lex derives structured knowledge directly from git's content-addressed storage.

## Repository structure

```
my-project/
├── .lex/
│   ├── schema/          # Kit type definitions (OWL/RDFS)
│   ├── extraction.log   # Which files have been extracted
│   ├── graphs/          # RDF graph files (N-Triples)
│   └── config.toml      # git-lex configuration
├── notes/               # Your documents (markdown)
├── contacts/
├── tasks/
└── ...
```

## Data flow

1. **Write** — Author markdown documents with YAML frontmatter
2. **Save** — `git lex save` commits and triggers extraction
3. **Extract** — Frontmatter properties become RDF triples; markdown links in document bodies become `linksTo` relationship triples, in the same pass
4. **Sync** — `git lex sync` builds the complete knowledge graph from git history + extractions
5. **Query** — `git lex query` runs SPARQL against the graph

## No binary stores

Everything in `.lex/` is text-based and git-diff-friendly. No SQLite, no RocksDB, no binary indexes. This means:

- Knowledge graphs can be merged with standard git merge
- Diffs show exactly what changed in the graph
- The entire knowledge state is versioned with your content
- Any git host (GitHub, GitLab, etc.) can serve as backup

## Namespaces

git-lex uses RDF namespaces to organize triples:

| Prefix | URI | Purpose |
|---|---|---|
| `git2:` | `https://repolex.ai/ontology/git-lex/git2/` | Git metadata (commits, authors, timestamps) |
| `git-lex:` | `https://repolex.ai/ontology/git-lex/` | Core properties shared by every type (`id`, `title`, `relatedToId`) |
| `fm:` | `https://repolex.ai/ontology/git-lex/fm/` | Frontmatter properties, on the file |
| `md:` | `https://repolex.ai/ontology/git-lex/md/` | Markdown body facts (`linksTo`, `externalLink`) |
| `soul:` | `https://repolex.ai/ontology/soul/` | Soul kit types and properties |

## Kits

Kits are pluggable type systems distributed as git-lex packages. A kit defines:

- **Document types** with typed properties
- **Templates** for `git lex create`
- **Schema** (OWL/RDFS) for validation and reasoning
- **Extraction rules** for custom property handling

The base kit is installed on every `init`. Domain kits are fetched from GitHub — `soul`, `squad`, `copia`, `lab` and others live under `repolex-ai/git-lex-kit-<name>` — and any `org/repo` publishing a kit can be installed the same way.
