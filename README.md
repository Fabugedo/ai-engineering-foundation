# AI Engineering Foundation

Reusable foundation for building professional software prototypes with Claude Code and Spec-Driven Development.

This repository is not a product application and does not impose a frontend framework, backend framework, database, cloud provider, AI provider, or deployment architecture.

Each concrete project created from this foundation must define those decisions through its own specification and technical plan.

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

## Included capabilities

### Spec-Driven Development

GitHub Spec Kit provides the workflow and artifacts for:

* project principles;
* feature specifications;
* clarification;
* technical planning;
* research;
* data models;
* contracts;
* executable tasks;
* consistency analysis;
* implementation.

The installed Claude Code commands include:

```text
/speckit-constitution
/speckit-specify
/speckit-clarify
/speckit-plan
/speckit-tasks
/speckit-checklist
/speckit-analyze
/speckit-implement
/speckit-converge
```

### Claude Code project instructions

`CLAUDE.md` defines permanent project rules for:

* requirements and assumptions;
* development workflow;
* architecture and scope;
* feature-oriented code organization;
* security and secret handling;
* destructive operations;
* database changes;
* frontend quality;
* testing and verification;
* dependency management;
* Git and publication.

### MCP servers

The project includes `.mcp.json` with:

| MCP      | Purpose                                               |
| -------- | ----------------------------------------------------- |
| Context7 | Current and version-sensitive technical documentation |
| Mermaid  | Editable technical and architecture diagrams          |

The repository configuration does not contain credentials.

MCP authorization is local to each developer and may be stored in files excluded from Git.

### Security and review

The project enables:

| Capability          | Purpose                                                    |
| ------------------- | ---------------------------------------------------------- |
| Gitleaks            | Deterministic secret scanning                              |
| insecure-defaults   | Review insecure configuration and unsafe defaults          |
| differential-review | Review meaningful Git changes and regressions              |
| `.gitignore`        | Exclude secrets, local state, reports, and generated files |
| `.env.example`      | Document environment variables without real credentials    |

### Frontend and browser verification

The project includes:

| Capability      | Purpose                                                             |
| --------------- | ------------------------------------------------------------------- |
| frontend-design | Improve interface composition, hierarchy, states, and accessibility |
| Playwright CLI  | Verify important flows in a real browser                            |

Playwright can be used to inspect:

* navigation;
* forms;
* visible states;
* responsive behavior;
* console errors;
* failed network requests;
* screenshots and snapshots.

### Git and GitHub

Git and GitHub CLI are used for:

* local version control;
* repository creation;
* commits;
* pushes;
* pull requests;
* issues and workflows when required.

The `/ship` project skill provides a controlled workflow for review, secret scanning, verification, commit, and push.

## Repository structure

```text
ai-engineering-foundation/
│
├── .claude/
│   ├── skills/
│   ├── settings.json
│   └── settings.local.json        # Local and excluded from Git
│
├── .playwright/                   # Local state and excluded from Git
│
├── .specify/
│   ├── extensions/
│   ├── integrations/
│   ├── memory/
│   ├── scripts/
│   ├── templates/
│   └── workflows/
│
├── .env.example
├── .gitignore
├── .mcp.json
├── CLAUDE.md
└── README.md
```

Concrete projects created from this foundation may later add:

```text
src/
tests/
migrations/
specs/
package.json
pyproject.toml
docker-compose.yml
```

Those files and directories are not imposed by the foundation.

## Code organization preference

When the selected framework permits it, product code should preferably be organized by business capability, feature, or bounded context.

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

Feature-specific API code, business logic, validation, persistence, types, and tests should remain close together.

Shared directories should contain only genuinely cross-cutting capabilities.

The approved technical plan or framework conventions may override this preference.

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

Some projects may require additional tools, but they should not be added to the foundation without a clear reusable purpose.

### Spec Kit scripts and PowerShell

This Spec Kit installation uses PowerShell scripts (`.specify/scripts/powershell/*.ps1`) to run the Spec-Driven Development workflow.

* Windows contributors can run these scripts with the built-in Windows PowerShell or PowerShell 7+.
* Linux and macOS contributors need PowerShell (`pwsh`, PowerShell 7+) installed and available on `PATH` to run the generated `.ps1` workflow scripts.

## Verify the local installation

Run:

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

Not every command is required by every project, but the foundation assumes its core tools are available locally.

## Starting a project

### Option 1: Copy the foundation

Copy the repository into a new project directory and remove any history that should not be inherited.

### Option 2: GitHub template repository

After the foundation is published and validated, enable the GitHub **Template repository** setting.

Then create each new project from the template.

### Initial project workflow

Inside the new project:

1. Open the project root in VSCode.
2. Start Claude Code from that root.
3. Authorize the required MCP servers locally.
4. Define or update the project constitution.
5. Create the first feature specification.
6. Clarify missing requirements.
7. Produce the technical plan.
8. Generate tasks.
9. Analyze consistency.
10. Implement and verify.

Do not build a concrete product directly inside the master foundation repository.

## Environment variables

`.env.example` contains safe, commented examples.

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

Useful command:

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

Database-specific tools and integrations belong to the concrete project unless they become a proven reusable need.

## Important boundaries

This foundation supports professional prototype development, but it does not automatically make a project production-ready.

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

## Current status

```text
Spec Kit                         Installed
Context7 MCP                     Configured
Mermaid MCP                      Configured
Playwright CLI skill             Installed
Frontend Design                  Enabled
insecure-defaults                Enabled
differential-review              Enabled
Gitleaks                         Installed; worktree scan passed
CLAUDE.md                        Configured
.gitignore                       Configured
.env.example                     Configured
README.md                        Configured
/ship skill                      Configured
Local Git repository             Initialized on main
Security plugin review           Pending
Initial Git commit               Pending
GitHub repository                Pending
Template repository setting      Pending
```


## Next foundation milestones

1. Run the configured security and differential review capabilities.
2. Stage and inspect the complete initial commit.
3. Create a clean initial commit.
4. Publish the repository privately.
5. Validate the repository from a fresh clone.
6. Enable the GitHub template setting when ready.


## Guiding principle

Add only capabilities with a clear, testable purpose.

More tools, plugins, MCP servers, dependencies, and abstractions do not automatically produce better software.

The preferred foundation is small enough to understand, strict enough to reduce avoidable mistakes, and flexible enough to support different prototype architectures.
