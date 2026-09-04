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

`git lex query` rebuilds its answer from the **working tree** every time it runs — it sees files
you have edited but not committed, and it never consults the stored graph. That makes it the
wrong tool for checking whether your graph is current: it always looks right.

If you query the store directly over HTTP (`git lex serve sparql`), remember that git-lex keeps
its data in **named graphs**. A bare `SELECT * WHERE { ?s ?p ?o }` reads only the default graph
and will come back with a handful of triples on a repository holding tens of thousands. Wrap the
pattern in `GRAPH ?g { ... }`, as above. `git lex query` unions the graphs for you, which is why
the same query behaves differently in the two places.

## Build the full graph

```bash
git lex sync
```

This rebuilds the knowledge graph from git history and all extractions. The graph is stored in `.lex/` and tracked by git.

**`git lex save` does not do this.** Saving commits your work and reconciles the extraction
sidecars; it does not advance the graph. So a repository can sit several commits ahead of the
graph built from it, and every number you get back will be internally consistent and describe
an earlier day.

To check, compare the newest commit the store was built at against `HEAD`. `git lex sync` will
tell you — it either reports `Already synced at <sha>` or does real work:

```bash
git lex sync
# Already synced at 7d8a36f8 (57.3ms)          <- graph is current
# Synced in 4379.9ms: 8591 quads; 1978 facts   <- graph was behind
```

Run `sync` before you trust a query you are going to act on.
