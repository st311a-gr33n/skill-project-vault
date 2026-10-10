# project-vault

An AI skill that scaffolds an [Obsidian](https://obsidian.md) / [Tolaria](https://github.com/refactoringhq/tolaria) **project vault** — persistent memory for a project, shared between humans and AI agents.

## What it does

`project-vault` initializes a `<project-name>-vault` directory inside an existing project that tracks architecture, components, decisions (ADRs), source-file indexes, progress, references, and a prioritized gap register. Each vault ships a controlled **tag vocabulary** for correlating notes across types and a **Gap Kanban** Obsidian Bases view that groups gaps by status. It also adds two merge-safe additions at the project root: a templated `README.md` and an Obsidian-workspace `.gitignore` block.

## What gets created

```
<project-root>/
├── README.md                          # project root (templated, never overwrites)
├── .gitignore                         # project root (obsidian block appended)
└── <project-name>-vault/
    ├── AGENTS.md                      # vault conventions + agent memory workflow
    ├── Home.md                        # project dashboard (canonical home note)
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

`templates/` is the canonical scaffold. Files contain `{{PLACEHOLDERS}}` that are substituted before writing.

## Requirements

Works with any AI tool that can load skills, including:

- opencode
- Claude Code
- any other agent that executes a skill's `SKILL.md`

No additional dependencies — files are scaffolded with plain file writes.

## Usage

Trigger the skill with phrases like *"init a project vault"*, *"create a project vault"*, or *"set up persistent memory"*.

The agent will gather the following inputs (instead of guessing):

- **Project name** — kebab-case (derived from the folder if not given)
- **Description** — one or two sentences
- **Language / stack**
- **Repo name** — defaults to the project folder name
- **Build/test command** — optional, defaults to `N/A`

See [`SKILL.md`](SKILL.md) for the full step-by-step workflow and placeholder table.

## Structure

| Path | Purpose |
|---|---|
| `SKILL.md` | Skill definition, workflow, and enforced conventions |
| `templates/vault/` | Canonical vault scaffold (substituted before writing) |
| `templates/vault/Gap Kanban.base` | Obsidian Bases kanban: `Gap` notes grouped by `status`, showing `type`, `priority`, `tags` |
| `templates/project-root/` | Root `README.md` template + `.gitignore` snippet |

## Conventions enforced

- Vault directory is always `<project-name>-vault` (lowercase, hyphenated).
- Wikilinks target **filenames**, never H1 titles.
- Note type lives in the `type:` frontmatter field; folders are human convenience only.
- `Gap` IDs (`GAP-001`…) and `Decision` IDs (`ADR-0001`…) are never reused.
- Gap `priority` ∈ `high, medium, low, feature`; `status` ∈ `Open, In Progress, Resolved, Won't Fix, Deferred`.
- Never edit an accepted ADR; write a new one that `supersedes` it.
- Every note carries at least one **domain tag** from a controlled vocabulary
  recorded in `AGENTS.md`; tags are lowercase kebab-case and correlate notes
  across types.
- `Gap Kanban.base` provides an Obsidian Bases kanban of gaps grouped by `status`.

## Maintainer notes

- The scaffold is canonicalized from the author's existing vaults (`Sombra-vault`, `flipper-badger_vault`); the older vaults are left untouched.
- Files are written directly with the `write` tool (portable, no Tolaria dependency for scaffolding). Tolaria MCP is used only for the optional attach/open step.
- Keep `SKILL.md`'s frontmatter `description` in sync with what this skill triggers on.

## License

_TODO: choose and state a license._
