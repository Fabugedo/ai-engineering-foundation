---
name: visual-audit
description: Audit an already-implemented web interface in a real browser and produce an evidence-backed, severity-ranked visual-quality report with a verdict, without changing app code or imposing an aesthetic.
allowed-tools: Read Glob Grep Bash(playwright-cli:*) Bash(npx:*) Bash(ls:*)
---

# Visual Audit

Use this skill when a web screen is already implemented and the user wants to know
whether it is not just working, but *well designed* — or asks to "audit the UI",
"review how this looks", "check the visual quality", or before approving a screen as done.

It complements, and does not replace:

- **`frontend-design`** — that skill *creates* distinctive UI; this one *evaluates* an
  existing render. Do not propose a new palette, typography system, or aesthetic here.
- **`playwright-cli`** — that skill *drives the browser*; this one *uses it* to gather
  evidence. Do not restate its commands; read
  [../playwright-cli/SKILL.md](../playwright-cli/SKILL.md) for how to open pages, snapshot,
  screenshot, resize, and read the console.
- **Spec Kit** (`speckit-*`) — the spec, plan, and acceptance criteria remain the source of
  truth for *what* the screen must do. This skill judges *how well the render communicates
  it*; it does not invent requirements, write tasks, or generate checklists.

## The question this skill answers

Distinguish three states and say which one the screen is in:

1. **It works** — the flow functions.
2. **It is tidy** — nothing is broken, but the layout is generic, weakly prioritized, or
   forgettable.
3. **It is well designed** — hierarchy, grouping, and composition actively help the user.

A screen that only reaches (1) or (2) is not "done" for a UI feature.

## Hard rules

- **No verdict without a real screenshot.** Every verdict must be backed by at least one
  actual capture of the render. You may complement screenshots with DOM snapshots or `eval`
  inspection, but captured pixels are mandatory.
- **Read-only.** Never modify application code, styles, or content during an audit.
- **Do not impose an aesthetic.** Never pick or mandate a palette, font pairing, framework,
  component library, or design system. Evaluate against design *principles* and the
  project's *own* existing design language.
- **If you cannot open the app or capture sufficient evidence, the verdict is
  `audit blocked` — never `approved`.**

## Procedure

1. **Establish the source of truth.** Read the feature's spec, plan, and acceptance criteria
   (and any `speckit-checklist` output). Note what the screen is for, who uses it, and its
   single primary job. If none exists, state your working assumption explicitly and continue.

2. **Define the audit scope.** List the screens, and for each the states to inspect —
   default, empty, loading, error, success, partial data — plus the **viewports** and themes
   that actually apply.
   - Use the viewports defined by the project's source of truth (spec, plan, design
     definition). Do not assume a fixed mobile-plus-desktop pair.
   - At minimum, review the product's primary viewport, plus one other relevant size when the
     product supports more than one.
   - Review mobile, tablet, laptop, or desktop widths only when they are within the product's
     actual supported scope.
   - Review light and dark themes only where they exist.

3. **Open the render.** Launch the app with `playwright-cli` (see its skill). If you do not
   have a URL or run command, do not declare `audit blocked` yet:
   - look for a documented URL or start command in the source of truth, `README`, plan, or
     project configuration;
   - use only commands that are already approved or documented for this project;
   - if there is still not enough information, ask the user for the URL or start command;
   - never invent commands, ports, or a stack.
   Only report `audit blocked` once the app genuinely cannot be reached or rendered after
   those steps.

4. **Capture evidence.** For every screen/state under audit, take at least one real
   screenshot to a git-ignored path (e.g. under `.playwright/`). Add DOM snapshots or `eval`
   reads where they clarify a finding (contrast values, focus order, element roles). Never
   commit evidence artifacts.

5. **Evaluate against the rubric** (below), across the in-scope states, viewports, and
   themes.

6. **Classify each finding** by **severity** and **category**, then write the report from
   `references/audit-report-template.md`.

7. **Assign the verdict** using the rules below.

## Rubric

Assess, at minimum:

- **Comprehension** — is it clear within seconds what the screen is and what to do?
- **Visual hierarchy** — does emphasis match importance?
- **Primary action** — is the main action obvious and singular?
- **Grouping** — is related information visually grouped; is unrelated information separated?
- **Composition & space** — alignment, rhythm, use of whitespace, balance.
- **Density** — too cramped or too sparse for the task?
- **Redundancy** — duplicated CTAs, repeated labels, competing emphasis.
- **Legibility** — type size, line length, contrast against background.
- **Consistency** — spacing scale, component styling, and vocabulary consistent across the
  screen and with the rest of the product.
- **States** — empty, loading, error, success, and partial-data states are designed, not
  afterthoughts.
- **Responsive** — layout holds at the reviewed viewports without overflow or collapse.
- **Light/dark** — both themes are legible and intentional, where they exist.
- **Visible accessibility** — visible focus, contrast, touch-target size, text scaling.
  (Report these; a full a11y audit is out of scope.)

## Findings: category and severity

Tag every finding with a **category**. The scope is visual quality, but call out the others
explicitly:

- **Visual** — hierarchy, composition, grouping, density, legibility, consistency
  (primary scope).
- **Functional** — something visibly broken in the render (overflow, dead control, missing
  state). Report it and hand behavioral verification to `verify` / Spec Kit; do not fix it
  here.
- **Accessibility (visible)** — focus, contrast, target size.

Severity:

- **Critical** — blocks the primary task or makes the screen unusable/unreadable for its
  audience.
- **Major** — significantly weakens comprehension, hierarchy, or the primary action; a
  reasonable user would struggle.
- **Minor** — noticeable rough edge that does not impede the task.
- **Info** — observation or optional polish.

## Recommendations

For each finding, give a **concrete, actionable** recommendation grounded in the render:
e.g. group the related fields into one card, remove the duplicate "Save" CTA, reduce the
visual weight of the secondary action, tighten the vertical rhythm, raise the contrast of the
primary button. Structural and compositional changes are in scope.

What is **out of scope**: choosing a specific palette, font, framework, library, or design
system. If the screen needs a genuine redesign or a new aesthetic direction, say so and hand
off to `frontend-design`. Frame every recommendation in terms of the project's existing
design language, not a new one.

## Verdict

- **approved** — zero Critical, zero Major, and zero Minor findings; only Info items, or none.
- **approved with observations** — one or more Minor findings, but zero Critical and zero
  Major.
- **requires another iteration** — at least one Critical or Major finding.
- **audit blocked** — sufficient evidence could not be obtained (the app could not be reached
  or rendered, or no real screenshot could be captured). Never downgrade this to approved.

Record the verdict and its justification in the report.
