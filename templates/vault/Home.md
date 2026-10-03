---
type: Project
status: Active
stage: Phase 0
repo: {{REPO}}
related_to:
  - "[[overview|Architecture Overview]]"
  - "[[current-state|Current State]]"
  - "[[gap-registry|Gap Registry]]"
tags:
  - {{PROJECT_NAME}}
---

# {{PROJECT_TITLE}} — Project Home

Dashboard and map of content for **{{PROJECT_TITLE}}**.

{{PROJECT_DESCRIPTION}}

This vault is the project's persistent memory for humans and AI agents. Start with
`progress/current-state.md`, then navigate the category views.

## Status

- **Stage:** `Phase 0` — vault initialized.
- **Stack:** {{STACK}}
- **Repo:** `{{REPO}}` (see `README.md` at the repository root).
- **Open gaps:** see [[gap-registry|Gap Registry]].

## Categories

| Category | View | Notes |
|---|---|---|
| Architecture | [[overview|Architecture Overview]] | `architecture/` |
| Components | `components/` | one module or package per note |
| Decisions | `decisions/` | ADR-0001 … |
| Source files | `files/` | per-file indexes |
| Progress | [[current-state|Current State]], [[phases|Phases]], [[changelog|Changelog]], [[worklog|Worklog]], [[sessions|Sessions]] | `progress/` |
| References | `references/` | papers, tools, links |
| Gaps | [[gap-registry|Gap Registry]] | [high](gaps-high) · [medium](gaps-medium) · [low](gaps-low) · [feature](gaps-feature) |

## Quick links

- [[overview|Architecture Overview]]
- [[current-state|Current State]]
- [[phases|Phases]]
- [[gap-registry|Gap Registry]]

## How to use this vault

- **Humans:** open it in Obsidian or Tolaria and browse the saved views.
- **Agents:** follow the workflow in `AGENTS.md`; read [[current-state|Current State]] first and
  append to [[worklog|Worklog]] after every session.
