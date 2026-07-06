---
name: adr
description: Scaffold and write an Architecture Decision Record (ADR) for a significant technical decision.
allowed-tools: Read Glob Grep Bash(ls:*) Bash(cat:*)
---

# Architecture Decision Record

Use this skill when the user approves a significant technical or architectural decision and
it should be recorded, or when the user asks to "write an ADR", "record this decision", or
"document why we chose X".

The purpose of an ADR is to capture **why** a decision was made — the reasoning and the
rejected alternatives — so that a future contributor or a later agent session does not
silently undo it. It complements Spec Kit; it does not duplicate it.

Read `docs/adr/README.md` and `docs/architecture-decision-guide.md` for the conventions
before writing.

## When an ADR is warranted

Write one when the decision:

* changes the architecture or the boundaries between modules;
* introduces, removes, or swaps an external dependency (database, external API, AI provider, authentication);
* establishes a data-flow or trust boundary;
* selects a framework, protocol, or storage approach;
* is expensive or painful to reverse.

If the change is small and reversible, tell the user an ADR is not needed rather than
creating noise.

## Procedure

1. **Confirm the decision is real and approved.** If the user is still deciding, first
   present two or three options with trade-offs and a recommendation, and let them choose.
   Do not record a decision the user has not made.

2. **Pick the next number.** List existing records:

   ```bash
   ls docs/adr/
   ```

   The next ADR is the highest existing number plus one, zero-padded to four digits
   (`0000-template.md` and the README do not count as decisions). Use a short kebab-case
   title: `docs/adr/0003-use-postgres-for-catalog.md`.

3. **Draft from the template.** Read `docs/adr/0000-template.md` and fill every section:
   Context, Options considered, Decision, Why, Consequences. Write in plain language a
   junior can follow. The **Why** and the rejected **Options** are the most valuable parts —
   do not skip them.

4. **Keep it to one page.** Honest and short beats long and vague. Do not restate how the
   code works; state why this path was chosen over the alternatives.

5. **Update the index.** Add a row to the table in `docs/adr/README.md`.

6. **Update the diagram if structure changed.** If the decision changes module boundaries,
   data flow, or external dependencies, update `docs/architecture.md` (Mermaid) to match.

7. **Never delete or rewrite an existing ADR.** If this decision reverses an earlier one,
   create a new ADR and set the old one's status to `Superseded by ADR-NNNN`, preserving the
   history of why the change was made.

## Completion standard

Report: the ADR file created, its number and title, whether the index and the architecture
diagram were updated, and any follow-ups the decision obligates. Do not finish with only
"done".
