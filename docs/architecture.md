# Architecture

> **Convention:** every concrete project keeps at least one current architecture diagram
> here. Update it whenever module boundaries, data flow, or external dependencies change —
> it is part of "done" for any feature that changes structure. Use Mermaid so the diagram
> is editable and reviewable in Git. This file in the foundation is a **template**; replace
> the placeholders when you start a real project.

## System / container view

*What are the modules, and which external services do they touch?*

```mermaid
flowchart TB
  subgraph Client["Client"]
    UI["<frontend app>"]
  end
  subgraph Server["<backend app>"]
    F1["<feature module 1>"]
    F2["<feature module 2>"]
    ADP["<external adapter, e.g. ai / payments>"]
  end
  DB[("<database>")]
  EXT["<external API>"]

  UI -->|"typed HTTP / contract"| F1
  UI --> F2
  F1 -->|"repository"| DB
  F2 --> ADP
  ADP -->|"HTTPS (untrusted, validated)"| EXT
```

## Main use-case data flow

*Trace the single most important request end to end. Mark where untrusted input is validated.*

```mermaid
flowchart LR
  A[User input] --> B[Validate at boundary]
  B --> C[Core / use case<br/>no external deps]
  C --> D[(Persistence)]
  C --> E[Response]
```

## Data model (if a database exists)

```mermaid
erDiagram
  ENTITY_A ||--o{ ENTITY_B : has
  ENTITY_A {
    int id PK
  }
```

## Boundaries & trust

- **Trusted:** <code you control and test>
- **Untrusted (validate before use):** <external API responses, uploaded files, AI output, scraped data>
- **Secrets live only in:** <which module reads env vars; never the client>

## Key decisions

See `docs/adr/` for the reasoning behind the structure above.
