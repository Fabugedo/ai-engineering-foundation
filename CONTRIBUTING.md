# Contributing

Thanks for your interest. This repository is a **reusable foundation** for building
professional prototypes with Claude Code and Spec-Driven Development — it is *not* a product.
Contributions should keep it that way: small, general, and safe.

## The one rule that shapes everything

Only add capabilities with a clear, testable, **reusable** purpose. Product-specific
frameworks, business rules, database schemas, providers, or integrations do **not** belong
here — they belong in the concrete projects created from this foundation. When in doubt, ask:
*"would this be useful in a completely different project?"* If not, it does not go in the
foundation.

## Workflow

Non-trivial changes follow the Spec-Driven Development flow already installed here
(`/speckit-specify` → `/speckit-clarify` → `/speckit-plan` → `/speckit-tasks` →
`/speckit-implement`). Small, low-risk fixes may use a lighter path but must still be
understood and verified.

For any change that affects architecture, module boundaries, an external dependency, or is
hard to reverse, record the reasoning as an ADR in `docs/adr/` (use the `/adr` skill). If a
change alters structure or data flow, update `docs/architecture.md`.

The engineering principles that govern all contributions live in
`.specify/memory/constitution.md`. Runtime working rules live in `CLAUDE.md`.

## Before you open a change

1. Keep the change focused; do not bundle unrelated edits.
2. Do not commit secrets. `.env` and local configuration stay out of Git; `.env.example`
   holds only fictional placeholders.
3. Run the safe publication flow with the `/ship` skill, which reviews the diff, scans for
   secrets with **gitleaks**, and runs applicable checks before committing and pushing.
4. Continuous Integration will also run gitleaks on every push and pull request
   (`.github/workflows/ci.yml`); a change must pass it.

## Commits

Use messages that describe intent, e.g. `docs: add architecture decision guide`,
`feat: add controlled repository shipping skill`, `fix: exclude local browser state`.
Avoid vague messages like `update`, `changes`, or `fix stuff`.

## Reporting issues

Open a GitHub issue describing what you expected, what happened, and how to reproduce it.
For anything security-sensitive, do not post secrets or exploit details in a public issue.
