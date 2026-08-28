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
git lex init --kit soul
```

This creates a `.lex/` directory in your repo with:
- Schema definitions for your kit's document types
- Extraction configuration
- An empty knowledge graph index

The base kit is always installed. The `--kit` flag adds a domain kit on top of it — `soul` for a personal knowledge repo, `squad` for a shared one, or `org/repo` for any kit published on GitHub. There is no default domain kit; if you omit `--kit` you get the base kit alone.

## Create your first document

```bash
git lex create note
```

This scaffolds a new markdown file with typed frontmatter:

```yaml
---
soul.Note.noteId: "my-first-note"
soul.Note.title: "Untitled Note"
soul.Note.topic: ""
---

Your content here.
```

Keys are flat and dotted — `kit.Class.property`, one per line. Each class folder holds a
`__<Class>.md` template listing every key that class declares, with its type and whether it
is required.

Edit the file, then save:

```bash
git lex save "my first note"
```

This stages all changes, commits, and runs extraction in one step.

## Query the graph

```bash
git lex query "SELECT ?doc ?topic WHERE {
  GRAPH ?g { ?doc a soul:Note ; soul:topic ?topic }
}"
```

Common prefixes are injected automatically — `git2:` (commits, authors), `git-lex:` (core properties), `fm:` (frontmatter), `md:` (markdown body), and your kit's own prefix, e.g. `soul:`.

## Build the full graph

```bash
git lex sync
```

This rebuilds the knowledge graph from git history and all extractions. The graph is stored in `.lex/` and tracked by git.
