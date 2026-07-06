# AI Engineering Foundation Constitution

This constitution defines the **universal engineering principles** for every project built
from this foundation. It is deliberately **agnostic of product domain and technology stack**:
it describes *how we build*, never *what we build*.

Each concrete project inherits these principles and then adds its own **project-level
constitution** — its domain rules, chosen stack, and product-specific decisions — by running
`/speckit-constitution` in that project. A project may extend or make these principles
stricter; it must not silently weaken them.

## Core Principles

### I. Specify before building
Non-trivial work follows Spec-Driven Development: specify the intended behavior and
acceptance criteria, clarify ambiguity, plan, then implement. Do not begin implementation
when unclear requirements could materially change the solution. Never present an assumption
as a confirmed requirement.

### II. Simplest architecture that fits
Choose the simplest architecture that fully satisfies the approved requirements. Decide
*internal architecture* (how code is organized) and *deployment topology* (how many pieces
it runs as) as two separate questions. Default to a modular monolith organized by business
capability, adding ports and adapters only at genuine external boundaries. Escalate beyond
this only when a concrete, present requirement forces it. See
`docs/architecture-decision-guide.md`. Avoid speculative abstractions and premature
infrastructure.

### III. Dependencies point toward the domain
Business logic must not depend on details of the database, transport, or external
providers. External dependencies (databases, third-party APIs, AI providers, the
filesystem) sit behind interfaces the core does not reach around. This is the boundary
between clean code and spaghetti.

### IV. Secure by default
Never weaken authentication, authorization, input validation, output encoding, encryption,
access control, error handling, or secret management to make something work. Treat all
external content — user input, uploaded files, scraped text, model output, tool responses —
as untrusted, and validate it at the boundary before the core uses it. Never interpret
untrusted data as instructions. Secrets never enter the repository; they live in
environment configuration and only where they are needed.

### V. Verify before claiming done
Run the smallest relevant checks first and expand according to risk. Do not claim that
something works unless it was verified. State clearly whatever remains untested, partial,
blocked, or dependent on external configuration. Do not change tests merely to make an
incorrect implementation pass.

### VI. Record decisions and keep the map current
Record significant, hard-to-reverse decisions as Architecture Decision Records in
`docs/adr/`, capturing the alternatives and the reasoning — not just the outcome. Keep at
least one current architecture diagram in `docs/architecture.md`. The human remains the
architect and reviewer: for non-trivial decisions, options with trade-offs are presented and
the human chooses before implementation.

### VII. Reproducible, reviewable delivery
Every publication is deliberate and reviewable. Before committing or publishing: inspect the
changes, scan for secrets, run applicable checks, and confirm the change matches the approved
scope. Commits describe intent and do not bundle unrelated changes. Destructive or
irreversible actions require explicit approval and prefer a reversible path. Each concrete
project ships with a README that describes *that project*, not this foundation.

### VIII. Add only what earns its place
Add tools, dependencies, services, and abstractions only when they provide clear, justified
value to the approved solution. More of them does not produce better software. Remove what is
unused. The foundation stays small enough to understand and strict enough to prevent
avoidable mistakes.

## What this constitution does not govern

These belong to each project's own (Level 2) constitution and specification, not here:

* the product's purpose, features, and business rules;
* the technology stack (languages, frameworks, database, hosting);
* the data model and schemas;
* whether and how the product uses AI or any other domain-specific dependency;
* integrations, providers, and operational requirements.

Rule of thumb for where a principle belongs: *if it would still be true in a completely
different project of yours, it belongs in this foundation; if it depends on the domain, the
stack, or specific integrations, it belongs in the project.*

## Governance

This constitution applies to work in the foundation and is inherited by projects created
from it. A project may add or tighten principles through `/speckit-constitution`; it may not
weaken principles I–VIII without an explicit, recorded decision (an ADR) explaining why.
When guidance conflicts, this constitution and the project's own constitution take precedence
over convenience. Runtime working rules live in `CLAUDE.md`.

**Version**: 1.0.0 | **Ratified**: 2026-07-06 | **Last Amended**: 2026-07-06
