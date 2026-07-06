# Architecture Decision Records (ADRs)

An **ADR** is a short note that records *one* significant technical decision and,
above all, **why** it was made. It is the single most effective habit for keeping a
project understandable, integrable, and free of "why on earth is this like this?"
moments — especially when the code is written by AI agents and read by someone else
(or by you, three months later).

An ADR is not documentation of *how the code works* — the code and the Spec Kit
artifacts already do that. An ADR captures the **reasoning behind a choice** so that
nobody silently undoes it.

## When to write one

Write an ADR when a decision:

- changes the **architecture** or the boundaries between modules;
- introduces, removes, or **swaps an external dependency** (database, external API, AI provider, auth);
- establishes a **data-flow or trust boundary** (what is trusted, what is validated where);
- picks a framework, protocol, or storage approach;
- is **expensive or painful to reverse**.

Do **not** write an ADR for small, reversible changes (renaming, a bug fix, a local
refactor). If reversing it later would be cheap, skip it.

Rule of thumb: *if a new teammate would ask "why did you do it this way?", it deserves an ADR.*

## How to write one

1. Copy `0000-template.md` to a new file.
2. Number it sequentially: `0001-...`, `0002-...`, using a short kebab-case title
   (`0003-use-postgres-for-catalog.md`).
3. Fill in Context → Options considered → Decision → Why → Consequences.
4. Keep it to one page. Shorter and honest beats long and vague.
5. Never delete an ADR. If a decision changes, write a *new* ADR and set the old one's
   status to `Superseded by ADR-XXXX`. The trail of *why we changed our mind* is valuable.

You can scaffold a new ADR with the `/adr` skill, which picks the next number for you.

## Working with an AI agent

Ask the agent to **propose 2–3 options with trade-offs and a recommendation before
implementing**, then have it write the ADR for the option you approve. This keeps you
in the architect's seat: you decide and understand the decision, the agent does the typing.

## Index

| ADR | Title | Status |
| --- | ----- | ------ |
| [0001](0001-record-architecture-decisions.md) | Record architecture decisions with ADRs | Accepted |
