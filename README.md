# AI Engineering Foundation

Reusable foundation for building professional software prototypes with Claude Code and Spec-Driven Development.

This repository is not a product application and does not impose a frontend framework, backend framework, database, cloud provider, AI provider, or deployment architecture.

Each concrete project created from this foundation must define those decisions through its own specification and technical plan.

## At a glance

A quick inventory of what ships in this repository.

| Area | What's included |
| ---- | --------------- |
| **Spec-Driven Development** | [GitHub Spec Kit](https://github.com/github/spec-kit), driven by PowerShell scripts under `.specify/` and exposed through the `speckit-*` skills. |
| **Skills** (15) | Spec Kit: `speckit-constitution`, `speckit-specify`, `speckit-clarify`, `speckit-plan`, `speckit-tasks`, `speckit-checklist`, `speckit-analyze`, `speckit-implement`, `speckit-converge`, `speckit-taskstoissues`, `speckit-agent-context-update`. Delivery: `adr`, `ship`, `playwright-cli`, `visual-audit`. |
| **MCP servers** (2) | Context7 (current technical documentation) and Mermaid (editable diagrams), configured in `.mcp.json` without credentials. |
| **Plugins** (3) | `frontend-design` (Anthropic), `insecure-defaults` and `differential-review` (Trail of Bits), enabled in `.claude/settings.json`. |
| **Documentation** | Architecture decision guide, ADRs, an editable architecture diagram, and a project README template under `docs/`. |

Each area is detailed in the sections below.

## Purpose

The foundation provides a disciplined starting point for AI-assisted development.

Its goals are to:

* reduce implementation based on unclear assumptions;
* keep requirements, plans, and tasks traceable;
* use current technical documentation;
* create editable architecture diagrams;
* review code and configuration for security issues;
* detect secrets before publication;
* verify important user flows in a real browser;
* keep product-specific decisions outside the reusable template.

## Core workflow

Non-trivial work follows GitHub Spec Kit:

```text
Constitution
    ↓
Specification
    ↓
Clarification
    ↓
Research
    ↓
Technical plan
    ↓
Executable tasks
    ↓
Consistency analysis
    ↓
Implementation
    ↓
Testing and review
    ↓
Publication
```

The scope of a prototype may be limited, but its requirements and implementation should still be defined professionally.

## Skills

Skills are the reusable procedures Claude Code can run inside this foundation. They live in
[.claude/skills/](.claude/skills/) and are invoked as `/<skill-name>`. Fifteen skills ship
with the foundation, grouped by purpose.

### Spec-Driven Development (GitHub Spec Kit)

These skills drive the core workflow, from defining principles to shipping code.

| Skill | Purpose |
| ----- | ------- |
| `/speckit-constitution` | Create or update the project constitution and keep dependent templates in sync. |
| `/speckit-specify` | Create or update a feature specification from a natural-language description. |
| `/speckit-clarify` | Ask targeted questions to resolve underspecified areas and record the answers in the spec. |
| `/speckit-plan` | Produce the technical plan and design artifacts from the spec. |
| `/speckit-tasks` | Generate a dependency-ordered `tasks.md` from the design artifacts. |
| `/speckit-checklist` | Generate a custom quality checklist for the current feature. |
| `/speckit-analyze` | Run a non-destructive consistency check across `spec.md`, `plan.md`, and `tasks.md`. |
| `/speckit-implement` | Execute the approved tasks in `tasks.md`. |
| `/speckit-converge` | Compare the codebase against the spec/plan/tasks and append any remaining unbuilt work as new tasks. |
| `/speckit-taskstoissues` | Convert tasks into dependency-ordered GitHub issues. |
| `/speckit-agent-context-update` | Refresh the managed Spec Kit section inside coding-agent context files. |

### Architecture and delivery

| Skill | Purpose |
| ----- | ------- |
| `/adr` | Scaffold and write an Architecture Decision Record for a significant technical decision (see [docs/adr/](docs/adr/)). |
| `/ship` | Safely review, scan, commit, and publish repository changes with Git and GitHub CLI. |
| `/playwright-cli` | Automate a real browser to verify web flows, forms, states, and responsiveness. |
| `/visual-audit` | Audit an already-implemented interface in a real browser and produce an evidence-backed, severity-ranked visual-quality report with a verdict. |

## Enabled plugins

Plugins are installed from external Claude Code marketplaces and enabled in
[.claude/settings.json](.claude/settings.json). Unlike skills, their source does not live in
this repository; they are pulled in per developer environment.

| Plugin | Marketplace | Purpose |
| ------ | ----------- | ------- |
| `frontend-design` | `claude-plugins-official` (Anthropic) | Distinctive, intentional UI design: aesthetic direction, typography, and layout that avoid templated defaults. |
| `insecure-defaults` | `trailofbits` | Review configuration and code for insecure defaults and unsafe settings. |
| `differential-review` | `trailofbits` | Review meaningful Git changes for regressions and defects. |

## Project instructions (`CLAUDE.md`)

[CLAUDE.md](CLAUDE.md) defines the permanent project rules Claude Code must follow, covering:

* requirements, assumptions, and decision labels;
* the development workflow;
* architecture and scope (paired with `docs/architecture-decision-guide.md`);
* feature-oriented code organization;
* security and secret handling;
* destructive and external actions;
* database changes;
* frontend quality;
* testing and verification;
* dependency management;
* change review, Git, and publication.

## MCP servers

[.mcp.json](.mcp.json) configures two documentation and diagramming servers. The repository
configuration contains no credentials; MCP authorization is local to each developer and stored
in files excluded from Git.

| MCP | Purpose |
| --- | ------- |
| Context7 | Current and version-sensitive technical documentation. |
| Mermaid | Editable technical and architecture diagrams. |

## Documentation and architecture

Reusable documentation lives in [docs/](docs/) and is meant to guide, not to be duplicated by
Spec Kit artifacts.

| File | Purpose |
| ---- | ------- |
| [docs/architecture-decision-guide.md](docs/architecture-decision-guide.md) | How to choose an internal architecture and a deployment topology, written for a junior to follow. |
| [docs/adr/](docs/adr/) | Architecture Decision Records — one decision per file, with `0000-template.md` and a `README.md` explaining the process. |
| [docs/architecture.md](docs/architecture.md) | The current architecture diagram(s) in Mermaid; a template to replace when a real project starts. |
| [docs/project-readme-template.md](docs/project-readme-template.md) | A copyable starting point for a concrete project's own README. |

> **Note:** the README a concrete project publishes must describe *that product*, not this
> foundation. Replace this file using `docs/project-readme-template.md` before the first
> publication of a real project.

## Repository structure

```text
ai-engineering-foundation/
│
├── .claude/
│   ├── skills/                     # The 15 skills documented above
│   ├── settings.json               # Enabled plugins
│   └── settings.local.json         # Local, excluded from Git
│
├── .github/                        # CI and repository workflows
│
├── .playwright/                    # Local browser state, excluded from Git
│
├── .specify/                       # Spec Kit engine
│   ├── extensions/
│   ├── integrations/
│   ├── memory/
│   ├── scripts/
│   ├── templates/
│   └── workflows/
│
├── docs/                           # Architecture guide, ADRs, diagrams, README template
│   ├── adr/
│   ├── architecture-decision-guide.md
│   ├── architecture.md
│   └── project-readme-template.md
│
├── .env.example
├── .gitignore
├── .mcp.json
├── CLAUDE.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

Concrete projects created from this foundation may later add source, tests, migrations, specs,
and package manifests (`src/`, `tests/`, `migrations/`, `specs/`, `package.json`,
`pyproject.toml`, `docker-compose.yml`). None of those are imposed by the foundation.

## Code organization preference

When the selected framework permits it, product code should preferably be organized by business
capability, feature, or bounded context.

Example:

```text
src/
├── features/
│   ├── registration/
│   ├── users/
│   ├── catalog/
│   └── dashboard/
│
├── shared/
│   ├── configuration/
│   ├── database/
│   ├── security/
│   ├── logging/
│   └── errors/
│
└── app/
```

Feature-specific API code, business logic, validation, persistence, types, and tests should
remain close together. Shared directories should contain only genuinely cross-cutting
capabilities. The approved technical plan or framework conventions may override this preference.

## What is intentionally not included

This foundation does not automatically include:

* React, Next.js, Vue, Angular, or another frontend framework;
* Express, NestJS, FastAPI, Django, or another backend framework;
* PostgreSQL or another database integration;
* Docker;
* cloud infrastructure;
* authentication providers;
* AI model providers;
* scraping services;
* vector databases;
* product-specific schemas or business rules.

These capabilities should be added only when required by a concrete project specification.

## Local prerequisites

Recommended local tools:

```text
Claude Code
Git
GitHub CLI
Gitleaks
Node.js
Playwright CLI
uv
Specify CLI
```

Some projects may require additional tools, but they should not be added to the foundation
without a clear reusable purpose.

### Spec Kit scripts and PowerShell

This Spec Kit installation uses PowerShell scripts (`.specify/scripts/powershell/*.ps1`) to run
the Spec-Driven Development workflow.

* Windows contributors can run these scripts with the built-in Windows PowerShell or PowerShell 7+.
* Linux and macOS contributors need PowerShell (`pwsh`, PowerShell 7+) installed and available on
  `PATH` to run the generated `.ps1` workflow scripts.

### Verify the local installation

```powershell
git --version
gh --version
gh auth status
gitleaks version
node --version
playwright-cli --help
uv --version
specify version
claude mcp list
```

Not every command is required by every project, but the foundation assumes its core tools are
available locally.

## Starting a project

### Option 1: Copy the foundation

Copy the repository into a new project directory and remove any history that should not be
inherited.

### Option 2: GitHub template repository

Once the foundation is published, enable the GitHub **Template repository** setting and create
each new project from the template.

### Initial project workflow

Inside the new project:

1. Open the project root in VSCode.
2. Start Claude Code from that root.
3. Authorize the required MCP servers locally.
4. Define or update the project constitution (`/speckit-constitution`).
5. Create the first feature specification (`/speckit-specify`).
6. Clarify missing requirements (`/speckit-clarify`).
7. Produce the technical plan (`/speckit-plan`).
8. Generate tasks (`/speckit-tasks`).
9. Analyze consistency (`/speckit-analyze`).
10. Implement and verify (`/speckit-implement`).
11. Replace this README using `docs/project-readme-template.md`.

Do not build a concrete product directly inside the master foundation repository.

## Environment variables

[.env.example](.env.example) contains safe, commented examples.

For a concrete project:

1. copy `.env.example` to `.env`;
2. add only variables required by the approved project plan;
3. place real local values only in `.env`;
4. never commit `.env`;
5. run Gitleaks before publication.

## Security workflow

Before committing or publishing changes:

```text
Review git status
    ↓
Inspect modified and untracked files
    ↓
Inspect the relevant diff
    ↓
Run applicable tests
    ↓
Run Gitleaks
    ↓
Review insecure defaults when relevant
    ↓
Run differential review for meaningful changes
    ↓
Confirm local files and secrets are excluded
    ↓
Commit and publish
```

The `/ship` skill runs this workflow in a controlled way. A useful manual command:

```powershell
gitleaks detect --source . --no-banner
```

A successful secret scan does not replace manual review.

## Database policy

No database engine is imposed by this foundation.

When a project requires persistence:

* select the database through the project specification;
* use versioned migrations;
* define constraints and indexes explicitly;
* use dedicated development credentials;
* avoid application access through a superuser;
* keep credentials outside Git;
* require approval for destructive migrations.

Database-specific tools and integrations belong to the concrete project unless they become a
proven reusable need.

## Important boundaries

This foundation supports professional prototype development, but it does not automatically make
a project production-ready.

Production systems may additionally require:

* infrastructure hardening;
* monitoring and alerting;
* automated backups;
* secret rotation;
* rate limiting;
* load testing;
* disaster recovery;
* audit logging;
* regulatory compliance;
* deployment security;
* operational documentation.

## Guiding principle

Add only capabilities with a clear, testable purpose.

More tools, plugins, MCP servers, dependencies, and abstractions do not automatically produce
better software.

The preferred foundation is small enough to understand, strict enough to reduce avoidable
mistakes, and flexible enough to support different prototype architectures.
