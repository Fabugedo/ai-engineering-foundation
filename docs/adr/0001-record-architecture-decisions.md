# ADR-0001: Record architecture decisions with ADRs

- **Status:** Accepted
- **Date:** 2026-07-06
- **Deciders:** Fabrizio
- **Related:** `docs/adr/README.md`, `docs/architecture-decision-guide.md`

## Context

Projects built from this foundation are implemented largely by AI agents (Claude Code)
under a Spec-Driven Development workflow. That produces good code fast, but it creates a
specific risk: **decisions get made without the human fully understanding or remembering
why.** The Spec Kit artifacts capture *what* to build; they do not durably capture *why a
particular technical path was chosen over the alternatives*. Without that record, a future
contributor — or an agent in a later session — can silently reverse a deliberate choice.

## Options considered

1. **Rely only on Spec Kit artifacts + commit messages** — no extra files, but the *why*
   is scattered and easily lost; commit messages rarely explain trade-offs.
2. **Write long design documents per feature** — thorough, but heavy, quickly stale, and
   duplicates Spec Kit content.
3. **Lightweight ADRs (one decision per one-page file)** — minimal overhead, durable,
   industry-standard, easy for both humans and agents to produce and read.

## Decision

We will record significant architecture and technology decisions as lightweight ADRs in
`docs/adr/`, one decision per file, using `docs/adr/0000-template.md`.

## Why

ADRs capture exactly the thing most at risk in agent-driven development — the reasoning —
at the lowest possible cost. They complement Spec Kit rather than duplicating it, they keep
the human in the architect's role, and they make a project genuinely *integrable* because
the next person can read the "why" instead of reverse-engineering it.

## Consequences

- **Good:** decisions are traceable; onboarding is faster; agents are instructed to propose
  options and record the chosen one, keeping the human in control.
- **Bad / cost:** a small, ongoing discipline — someone must actually write the ADR when a
  real decision is made.
- **Follow-ups:** the `/adr` skill scaffolds new records; `CLAUDE.md` requires an ADR for
  qualifying decisions; the decision guide informs which option to pick.
