# Product brief

## Purpose

A lightweight personal knowledge workspace that preserves learning and context over time. It should make it easy to capture interesting material, connect it to earlier learning and current projects, and resume work after interruptions.

## Primary user needs

1. Capture a title, URL, file, thought, or question quickly.
2. Revisit saved sources and personal notes.
3. Find related topics and understand how ideas connect or change.
4. Link research to projects, plans, decisions, and tasks.
5. Resume work with objective, current state, prior decisions, blockers, and next action.
6. Record completed work and attach documents or other evidence.
7. Ask questions about selected knowledge using evidence-grounded AI, only when explicitly requested.

## Product principles

- **Capture first, process selectively:** do not require AI processing at capture time.
- **Manual usefulness:** core capture, search, editing, linking, and project tracking work without AI.
- **Obsidian-first:** existing Markdown remains the authoritative content source for vault-origin notes.
- **Review before write-back:** application edits to vault-origin notes are pending proposals, never silent overwrites.
- **Stable identifiers:** titles and paths can change without changing identity.
- **Traceability:** extracted claims and relationships retain their source evidence and origin.
- **Lean interface:** primary pages are Home, Knowledge, Projects & Tasks, and optional Connections.
- **No unapproved automation:** AI suggestions do not become confirmed facts or actions without user acceptance.
- **Local-first:** initial development runs on the user's machine with Docker Compose and `.env` configuration.

## Main pages

### Home
Quick capture, inbox items, active projects, next actions, recently visited records, and sync health. Avoid vanity analytics, streaks, and unsolicited AI digests.

### Knowledge
Search and browse sources and notes; read captured content; edit personal annotations; view tags, topics, relationships, and linked projects; manually request extraction, summary, comparison, or Q&A.

### Projects & Tasks
Project objective, current state, plans, open questions, tasks, next action, activity history, decisions, and evidence. Provide a resume brief from stored records; AI-assisted generation remains user-triggered.

### Connections
Optional graph/list view for exploring and editing relationships. List view must remain available for clarity and accessibility.

## Capture fields

Required fields should be minimal. A title or content is sufficient for a manual note; a source URL may supply a title where available.

Optional fields:
- source URL / source type
- reason for saving
- personal note / interpretation
- question to answer
- linked project
- user-selected tags
- knowledge workflow state

Use progressive disclosure: show task-specific fields for tasks and source-specific fields for articles. Knowledge workflow state and task status are different concepts.

## Out of scope for the initial core

- Autonomous background research or AI processing
- Multi-agent orchestration
- Mandatory graph navigation
- Complex calendar and notification systems
- Gamification or productivity scoring
- Automatic destructive entity merging
- Mandatory Obsidian replacement
- Separate graph database before PostgreSQL traversal is shown to be insufficient

## Success criteria

- Capture can be completed in seconds.
- Saved items remain findable without AI.
- Sync is idempotent and recoverable after restart or temporary database outage.
- A project can be resumed from its stored objective, last known state, decisions, evidence, and next action.
- AI output is traceable to sources and can be rejected or edited.
- The user can understand and repair sync failures from diagnostics.
