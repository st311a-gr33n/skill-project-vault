---
name: project-vault
description: Use when the user wants to initialize a new Obsidian/Tolaria project vault (persistent memory for a project), e.g. "init a project vault", "create a project vault", "start a vault for this project", "set up persistent memory". Scaffolds a <project-name>-vault directory inside the project with AGENTS.md, note types, saved views, a tag vocabulary for cross-note correlation, a gap kanban base, progress/gap tracking, a project README.md, and an Obsidian-workspace-ignoring .gitignore.
---

# project-vault

Initialize a Tolaria/Obsidian **project vault** inside an existing project so that
humans and AI agents share persistent memory: architecture, components, decisions
(ADRs), source-file indexes, progress, references, and a prioritized gap register.

The vault is a new directory named `<project-name>-vault` inside the project, plus
two merge-safe additions at the project root (`README.md` and an Obsidian-workspace
`.gitignore` block).

## What gets created

```
<project-root>/
├── README.md                          # project root (templated, never overwrites)
├── .gitignore                         # project root (obsidian block appended)
└── <project-name>-vault/
    ├── AGENTS.md                      # vault conventions + agent memory workflow
    ├── Home.md                        # Project dashboard (canonical home note)
    ├── .gitignore                     # ignores .obsidian/workspace*, .trash/, caches
    ├── .obsidian/                     # app.json, appearance.json, core-plugins.json, graph.json
    ├── architecture/overview.md
    ├── components/  decisions/  files/  references/  attachments/   # empty (gitkeep)
    ├── gaps/gap-registry.md
    ├── progress/{current-state,phases,changelog,worklog,sessions}.md
    ├── types/                         # one template note per entity type
    ├── views/                         # 13 saved-view YAMLs
    └── Gap Kanban.base                # Obsidian Bases kanban: gaps grouped by status
```

`templates/` in this skill directory is the canonical scaffold. Files contain
`{{PLACEHOLDERS}}` that must be substituted before writing.

## Placeholders

| Placeholder | Meaning |
|---|---|
| `{{PROJECT_NAME}}` | kebab-case project name (e.g. `flipper-badger`) |
| `{{VAULT_NAME}}` | `{{PROJECT_NAME}}-vault` |
| `{{PROJECT_TITLE}}` | human-readable title (e.g. `Flipper Badger`) |
| `{{PROJECT_DESCRIPTION}}` | one or two sentences describing the project |
| `{{STACK}}` | language / framework / key dependencies |
| `{{REPO}}` | repo name (for the `repo:` frontmatter field) |
| `{{PROJECT_PATH}}` | absolute path to the project working folder |
| `{{BUILD_CMD}}` | build/test command, or `N/A` if none |

## Workflow

1. **Gather inputs** (ask the user, don't guess):
   - Project name (derive a kebab-case name from the folder if not given).
   - One-line/two-line description.
   - Language/stack.
   - Repo name (default to the project folder name).
   - Build/test command (optional; default `N/A`).

2. **Resolve the project root** — the current working directory unless the user
   gives another path. Confirm it exists.

3. **Create the vault directory** at `<project-root>/<project-name>-vault/`.

4. **Copy the scaffold**: copy `templates/vault/` recursively into the new vault
   directory, then substitute every `{{PLACEHOLDER}}` in every copied file
   (frontmatter and body). Use the placeholder table above. If `{{BUILD_CMD}}` is
   `N/A`, delete the "Build/test" step (item 4) from `AGENTS.md` and note "no
   build command" rather than leaving a bare `N/A`.

5. **Establish the tag vocabulary** — derive a small set of project-specific
   domain tags from the stack and description (e.g. `nfc`, `qr`, `app-ui`) and
   write them into the Domain row of the `## Tags` section in `AGENTS.md`.
   Confirm every `types/*.md` uses `tags: [type]`. Tags are the primary means of
   correlating notes across types, so this vocabulary must exist from the start.

6. **Project root README** — write `<project-root>/README.md` from
   `templates/project-root/README.md`. If a README already exists, ask before
   overwriting; otherwise leave it and just note the vault in it.

7. **Project root .gitignore** — append the block from
   `templates/project-root/gitignore-obsidian-snippet` to `<project-root>/.gitignore`
   (create it if missing). Only add lines that are not already present; never
   clobber existing rules.

8. **Mount & open** — if Tolaria MCP tools are available, attach the new vault via
   `tolaria_attach_vault` (absolute path) and open `Home.md` with
   `tolaria_open_note`. Skip gracefully if those tools are absent.

9. **Summarize** — report the vault path, the files created, and the "next steps":
   open the vault in Obsidian/Tolaria, and agents should read
   `progress/current-state.md` first.

## Conventions enforced

- Vault directory name is always `<project-name>-vault` (lowercase, hyphenated).
- Wikilinks target **filenames**, never H1 titles.
- Note type lives in the `type:` frontmatter field; folders are human convenience only.
- `Gap` IDs (`GAP-001`…) and `Decision` IDs (`ADR-0001`…) are never reused.
- Gap `priority` ∈ `high, medium, low, feature`; `status` ∈ `Open, In Progress, Resolved, Won't Fix, Deferred`.
- Never edit an accepted ADR; write a new one that `supersedes` it.
- Every note carries at least one **domain tag** from the controlled vocabulary in
  `AGENTS.md`; tags are lowercase kebab-case and reused before invented. Tag each
  gap with the domain tags of the subsystem it affects so gaps correlate with the
  rest of the vault.
- `Gap Kanban.base` is an Obsidian Bases kanban that filters `type == "Gap"`,
  groups by `status`, and displays `type`, `priority`, and `tags`.
- The vault's own `.gitignore` ignores `.obsidian/workspace*` and related UI state.

## Notes

- This scaffold is canonicalized from the author's existing vaults
  (`Sombra-vault`, `flipper-badger_vault`); the older vaults are left untouched.
- Write files directly with the `write` tool (portable, no Tolaria dependency for
  scaffolding). Use Tolaria MCP only for the optional attach/open step.
