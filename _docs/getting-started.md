---
title: Getting Started
order: 1
nav_title: Getting Started
---

## Installation

git-lex is a Rust binary that installs as a git subcommand.

```bash
cargo install git-lex
```

Once installed, all commands are available as `git lex <command>`.

## Initialize a repository

```bash
cd my-project
git lex init --kit solo
```

This creates a `.lex/` directory in your repo with:
- Schema definitions for your kit's document types
- Extraction configuration
- An empty knowledge graph index

The `--kit` flag selects which document type system to use. The **solo** kit is the default, designed for personal knowledge management.

## Create your first document

```bash
git lex create note
```

This scaffolds a new markdown file with typed frontmatter:

```yaml
---
title: "Untitled Note"
tags: []
solo:
  type: Note
  topic: ""
  relatedTo: ""
---

Your content here.
```

Edit the file, then save:

```bash
git lex save "my first note"
```

This stages all changes, commits, and runs extraction in one step.

## Query the graph

```bash
git lex query "SELECT ?name ?type WHERE {
  GRAPH ?g { ?doc fm:solo.type ?type ; fm:title ?name }
}"
```

Prefixes `git:`, `fm:`, `lex:`, `lex-o:`, and `solo:` are auto-injected.

## Build the full graph

```bash
git lex sync
```

This rebuilds the knowledge graph from git history and all extractions. The graph is stored in `.lex/` and tracked by git.
