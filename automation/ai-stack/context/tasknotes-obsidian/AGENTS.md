# Design Document: Obsidian Second Brain

## Overview
This document outlines the architectural understanding and tagging strategy for the Obsidian vault located at `/mnt/backup/syncthing/notes/obsidian-tasknotes`. This vault serves as a "Second Brain," acting as a central repository for knowledge, project management, and personal development. Task management within the vault is handled by the **TaskNotes** plugin, which manages its own `TaskNotes/` directory at the vault root. For task schema, CLI, and API details, see the `tasknotes` skill — don't duplicate that reference here.

## Organizational Structure
The vault follows the **PARA method** (Projects, Areas, Resources, Archives), providing a clear distinction between active work and long-term reference material:

*   **0-Inbox**: Raw, unprocessed notes and initial captures.
*   **1-Projects**: Active endeavors with a specific goal and end date (e.g., Learning Goals, Shopping).
*   **2-Areas**: Ongoing responsibilities and domains of interest that require long-term maintenance (e.g., Dev, Education, Homelab).
*   **3-Resources**: Collections of interests, research, and reference material (e.g., Blog notes, Career paths, CCNA, Prompt Library).
*   **4-Archives**: Completed projects, inactive areas, and historical references.

**`TaskNotes/`** sits alongside these five folders at the vault root — it is not a numbered PARA folder and is auto-managed by the plugin, not manually organized:
*   **TaskNotes/Tasks/**: individual task notes (one file per task), created here automatically regardless of which PARA area they relate to.
*   **TaskNotes/Views/**: Bases-powered view definitions (Task List, Kanban, Calendar, Mini Calendar) — plain `.base` files, safe to inspect but not meant to be reorganized into PARA.

## Task Model (TaskNotes) — vault-specific notes only
This vault runs TaskNotes on **default settings** — default statuses (`open`/`in-progress`/`done`), default priorities (`low`/`normal`/`high`), default `tags: [task]`. Don't assume custom values; check an existing task file if in doubt. Full frontmatter schema, required fields, recurrence, and dependency syntax are covered by the `tasknotes` skill.

**Note on `dateCreated`/`dateModified`:** this vault's TaskNotes install writes these as full ISO 8601 timestamps with timezone offset (e.g. `2026-05-31T10:49:12.895-07:00`), not bare `YYYY-MM-DD`. Match this format on any task you create or edit, even if other guidance suggests otherwise.

**PARA relationship:** a task's physical location does not need to match its PARA folder. All tasks live under `TaskNotes/Tasks/`; the `projects:` field (a `[[wikilink]]` to the relevant note in `1-Projects/` or `2-Areas/`) is what ties a task back into PARA. Don't move task files into PARA folders manually.

### Tag synergy with the Graph View strategy
Task `tags:` and note `tags:` are the same underlying field. A task tagged with one of the granular topic tags from the Tagging Strategy below (e.g. `#terraform`, `#aws`) automatically shows up in that topic's thematic cluster in Graph View *and* is filterable as a task by that same tag — no extra bookkeeping needed. When tagging a task, prefer the existing granular topic tags over inventing task-only tags, so this overlap keeps working.

## Tagging Strategy & Graph View Goals

The primary objective for implementing tags is to enhance the **Graph View** to reveal hidden relationships and structural clusters that folder-based organization alone cannot show.

### Tagging Format
Tags should be implemented in the **YAML frontmatter** using the `tags:` key (e.g., `tags: [aws, terraform]`), following the format used at the top of `AGENTS.md`. Do not use hashtags within the body text unless referring to them as examples.

### Core Principles
1.  **Thematic Clustering**: Tags should be used to group notes into distinct thematic "islands" in the graph view (e.g., grouping all AI-related notes together, even if they reside in different folders like `Dev/ideas` and `Resources/blog notes`).
2.  **Granularity**: Use specific technology and topic-based tags rather than broad categories. Instead of just `#dev`, use `#terraform`, `#ansible`, `#docker`, and `#kubernetes`.
3.  **Multi-dimensional Linking (Multi-tagging)**: Notes should often carry multiple tags to represent cross-cutting relationships. A note on "Automating AWS with Terraform" should contain both `#aws` and `#terraform` to ensure it appears in both thematic clusters in the graph.
4.  **No Entity Tracking (Current Scope)**: The current focus is on topical/technological themes rather than tracking specific entities like people or hardware components.

### Identified Primary Themes for Tagging
Based on initial inspection, the following domains are prime candidates for granular tagging:
*   **AI & LLMs**: `#ai`, `#llm`, `#prompt-engineering`, `#agentic-workflows`.
*   **DevOps & Infrastructure as Code (IaC)**: `#terraform`, `#ansible`, `#docker`, `#kubernetes`, `#ci-cd`.
*   **Networking & Homelab**: `#networking`, `#homelab`, `#netbird`, `#vpn`, `#rsync`, `#proxmox`.
*   **Cloud Services**: `#aws`, `#oci`.
*   **Career & Learning**: `#career-development`, `#learning-python`, `#ccna`, `#wgu`.
*   **Linux/System Administration**: `#linux`, `#fedora`, `#kde`, `#bash`.

## Future Evolution
As the vault grows, this design document will be updated to reflect new themes, shifts in organizational methodology, or changes in how information is synthesized.

---

## Rules to follow
- You may use the printf bash command to add/edit tags
- Always keep your responses concise to the user
- Ask clarifying questions if the user's intent is ambiguous
- If something, such as editing a shell script that is being used as a test or tool, would be better off edited to your preference, go ahead and change it. For example, if you think the script can be more efficient with a different approach, go ahead and change that script for yourself. Do not make entire new scripts.
- Frontmatter-only edits to a task (status, priority, due date) are routine; don't rewrite a task's body or delete a task file without asking first.
- Don't invent status or priority values for a task — match whatever the vault is already using.
- The MCP server should be tested for interacting with TaskNotes before any other method.
- For full TaskNotes CLI/API/schema details, see the `tasknotes` skill rather than duplicating that reference here.
