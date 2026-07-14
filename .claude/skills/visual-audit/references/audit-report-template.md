# Visual Audit Report — <Screen / Feature>

> Fill every placeholder, then delete this quote block.

## Summary

- **Screen / feature:** <name>
- **Source of truth:** <spec / plan / acceptance criteria referenced, or "assumption: ...">
- **Audience & primary job:** <who / the single task the screen must serve>
- **Date:** <YYYY-MM-DD>
- **Verdict:** <approved | approved with observations | requires another iteration | audit blocked>

## Scope inspected

| Dimension | Inspected |
| --------- | --------- |
| States | <default / empty / loading / error / success / partial — mark which> |
| Viewports | <the project's primary viewport, plus any other in-scope size, e.g. 1440 desktop> |
| Themes | <light / dark / not applicable> |

## Evidence

All artifacts are git-ignored and never committed. At least one real screenshot is required;
snapshots and DOM reads are optional complements.

| # | What it shows | Path |
| - | ------------- | ---- |
| 1 | <state / viewport / theme> | <.playwright/...png> |

## Findings

| ID | Severity | Category | Observed problem | Evidence | User impact | Recommendation |
| -- | -------- | -------- | ---------------- | -------- | ----------- | -------------- |
| F1 | <Critical/Major/Minor/Info> | <Visual/Functional/Accessibility> | <what is wrong> | <screenshot # / region> | <why it matters to the user> | <concrete visual or structural change> |

## Verdict & justification

**<verdict>** — <one short paragraph referencing the finding counts.
`approved` requires zero Critical, zero Major, and zero Minor findings (only Info, or none).
`approved with observations` allows Minor findings but zero Critical and zero Major.
Any Critical or Major finding forces `requires another iteration`.
If sufficient evidence could not be obtained, the verdict is `audit blocked`.>

## Handoffs (if any)

- Redesign / new aesthetic direction → `frontend-design`
- New work to build → `speckit-tasks` / `speckit-converge`
- Behavioral verification → `verify`
