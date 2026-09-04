---
title: Document Types
order: 2
nav_title: Document Types
---

git-lex uses **kits** to define document types. Each kit provides a set of templates with typed frontmatter properties.

## The soul kit

The **soul** kit is designed for a personal knowledge repository. Install it with:

```bash
git lex init --kit soul
```

Kits are fetched from GitHub at `init` time; the base kit is always installed underneath.

### How frontmatter is written

Keys are flat and dotted — `kit.Class.property`, one per line. There is no nested block and
no `type:` key; the class is part of every key name.

```yaml
---
soul.Note.noteId: "thing-plane-first-flight"
soul.Note.title: "Thing plane, first flight"
soul.Note.topic: "the two-plane model"
---
```

Every class folder holds a `__<Class>.md` template listing every key that class declares,
its type, and whether it is required. That template is generated from the ontology, so it is
always current — read it rather than this page if the two ever disagree.

### Properties every class has

| Property | Type | Description |
|---|---|---|
| id | IRI | This document's identity, in angle brackets — `<soul/Note/my-note>` |
| title | string | Human-readable name |
| description | string | One-line summary |
| abstract | string | Longer summary |
| cue | string | When this document is worth recalling; repeatable |
| relatedToId | IRI | Reference to another document; repeatable |
| dateCreated | datetime | |
| dateUpdated | datetime | |
| substrate | string | |
| fileId | IRI | The file this document is written in |

### Note

Anything with no other home.

| Property | Type | Description |
|---|---|---|
| noteId | string | Identifier slug |
| topic | string | What this note is about |

### Journal

One entry per waking — the thread of your days.

| Property | Type | Description |
|---|---|---|
| journalId | string | Identifier slug |
| earthDate | date | **Required.** The calendar date this entry covers |
| soulDay | integer | **Required.** The soul-day number. Order by this, not by `earthDate` — two wakings can share a calendar day |
| emojimood | string | The day's vibe as emoji |

### Memory

Facts, preferences and lessons worth keeping.

| Property | Type | Description |
|---|---|---|
| memoryId | string | Identifier slug |
| confidence | enum | `certain`, `likely`, `hypothesis`, `hunch` |
| category | enum | `fact`, `lesson`, `observation`, `preference`, `decision`, `reference`, `skill` |
| source | string | Where this came from |

### Exploration

A thread of focused inquiry, usually anchored to a Pursuit.

| Property | Type | Description |
|---|---|---|
| explorationId | string | Identifier slug |
| explorationStatus | enum | `exploring`, `active`, `paused`, `concluded` |
| hypothesis | string | What you're trying to figure out |
| findings | string | What you've discovered so far |

### Pursuit

A standing thread you're advancing. Explorations point at these.

| Property | Type | Description |
|---|---|---|
| pursuitId | string | Identifier slug |

### Skill

A procedure you can run by name.

| Property | Type | Description |
|---|---|---|
| skillId | string | **Required.** Identifier slug |
| skillDescription | string | **Required.** What it does and when to reach for it |
| skillInvocability | enum | `user`, `agent`, `both` |
| skillAllowedTools | string | Tools the skill may use |
| skillArgumentHint | string | Argument shape, for help text |

### Soul

Your identity. One per repository, and it lives at the **repository root** as `SOUL.md`,
not in a class folder.

| Property | Type | Description |
|---|---|---|
| soulId | string | Derived from the genesis commit. The required floor |

## References between documents

`relatedToId` takes angle-bracket notation — `<namespace/Class/identifier>`:

```yaml
soul.Exploration.relatedToId:
  - <soul/Pursuit/legible-knowledge-graphs>
```

The identifier is the document's own declared slug, not its filename. Where the two differ,
the declared slug wins — a reference built from a filename can resolve to the wrong document
rather than failing, which is much harder to notice.

## Relationships from the body

Standard markdown links in a document body become `linksTo` relationships:

- `[display text](/Soul/Note/some-doc.md)` creates a `md:linksTo` triple to that document
- paths are root-relative, so they survive the linking file moving

These are extracted automatically, in the same pass that reads the frontmatter. git-lex does
not read `[[wikilinks]]`; a bracketed name in a document body is plain prose.

Note the name: `md:linksTo` has no `Id` suffix, because it is not derived from a declared
identifier — it is parsed out of the markdown body, which is what the `md/` segment marks.
Every `...Id` predicate names the id its value derives from, and a body link has no such id
to name.

## Deprecated types

Earlier versions of the soul kit shipped Contact, Task, Decision, Research and others. Those
classes are deprecated — still declared so existing documents keep working, but they are not
recommended for new work and are not listed above. If you are reading an ontology directly,
filter `owl:deprecated` or you will build against retired terms.
