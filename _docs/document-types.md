---
title: Document Types
order: 2
nav_title: Document Types
---

git-lex uses **kits** to define document types. Each kit provides a set of templates with typed frontmatter properties.

## Solo Kit

The solo kit ships with git-lex and is designed for personal knowledge management. Install it with:

```bash
git lex init --kit solo
```

### Note

General-purpose notes with topic linking.

```yaml
solo:
  type: Note
  topic: "knowledge graphs"
  relatedTo: "[[other-document]]"
```

| Property | Type | Description |
|---|---|---|
| topic | string | Primary topic |
| relatedTo | reference | Link to related document |

### Contact

People and organizations.

```yaml
solo:
  type: Contact
  contactType: "person"
  relationship: "collaborator"
  squad: "engineering"
  email: "name@example.com"
```

| Property | Type | Description |
|---|---|---|
| contactType | string | person, org, team |
| relationship | string | Nature of relationship |
| squad | string | Team or group |
| email | string | Email address |
| notes | string | Freeform notes |

### Task

Trackable work items.

```yaml
solo:
  type: Task
  taskStatus: "active"
  priority: "high"
  blockedBy: "[[other-task]]"
  project: "git-lex"
```

| Property | Type | Description |
|---|---|---|
| taskStatus | string | active, done, blocked, backlog |
| priority | string | high, medium, low |
| blockedBy | string | Blocking dependency |
| project | string | Project name |

### Decision

Architectural and strategic decisions.

```yaml
solo:
  type: Decision
  alternatives: "Option A, Option B"
  rationale: "Option A has better performance"
  outcome: "Chose Option A"
```

| Property | Type | Description |
|---|---|---|
| alternatives | string | Options considered |
| rationale | string | Reasoning |
| outcome | string | What was decided |
| supersededBy | reference | Later decision that replaced this one |

### Research

Investigation and analysis.

```yaml
solo:
  type: Research
  researchStatus: "in-progress"
  hypothesis: "RDF scales for code graphs"
  findings: ""
```

| Property | Type | Description |
|---|---|---|
| researchStatus | string | in-progress, complete, abandoned |
| hypothesis | string | What you're investigating |
| findings | string | Results |
| references | string | Sources |

### Memory

Things to remember — facts, preferences, context.

```yaml
solo:
  type: Memory
  confidence: "high"
  source: "direct observation"
  category: "technical"
```

| Property | Type | Description |
|---|---|---|
| confidence | string | high, medium, low |
| source | string | Where this came from |
| category | string | Topic category |

## Relationships

Use `@mentions` and `[[wikilinks]]` in document bodies to create relationships:

- `@agentname` creates a `lex:mentions` triple
- `[[document-title]]` creates a `lex:linksTo` triple

These are extracted automatically from both document content and commit messages.
