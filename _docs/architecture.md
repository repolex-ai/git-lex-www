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
| `git:` | `https://repolex.ai/ontology/git/` | Git metadata (commits, authors, timestamps) |
| `fm:` | `https://repolex.ai/ontology/frontmatter/` | Frontmatter properties |
| `lex:` | `https://repolex.ai/ontology/lex/` | git-lex relationships (mentions, linksTo) |
| `lex-o:` | `https://repolex.ai/ontology/lex-o/` | git-lex ontology classes |
| `solo:` | `https://repolex.ai/ontology/solo/` | Solo kit types and properties |

## Kits

Kits are pluggable type systems distributed as git-lex packages. A kit defines:

- **Document types** with typed properties
- **Templates** for `git lex create`
- **Schema** (OWL/RDFS) for validation and reasoning
- **Extraction rules** for custom property handling

The solo kit is built-in. Custom kits can be installed from git repositories.
