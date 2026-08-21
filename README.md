# GIGA / Jarvis sovereign architecture

This is the documentation-only home for the complete GIGA architecture map. It describes the
whole system: Jarvis as the persistent engine, the optional Personal Vault and digital-twin
payload, deterministic policy enforcement, routing, evidence and memory, autonomous initiative,
runtime continuity, and governed adapters to independently owned work systems.

## Documents

1. [`11_JARVIS_SOVEREIGN_RUNTIME_PROPOSED_ARCHITECTURE.md`](docs/11_JARVIS_SOVEREIGN_RUNTIME_PROPOSED_ARCHITECTURE.md)
   defines the end-state system, requirements, layers, ownership boundaries and architectural laws.
2. [`12_JARVIS_SOVEREIGN_RUNTIME_IMPLEMENTATION_BLUEPRINT.md`](docs/12_JARVIS_SOVEREIGN_RUNTIME_IMPLEMENTATION_BLUEPRINT.md)
   decomposes that map into independently useful implementation slices with prerequisites,
   verification gates, rollback rules and restart instructions.

## Architecture at a glance

```mermaid
flowchart TB
    O["Artiom"] -->|"goals · Constitution · delegation · manual work"| J

    subgraph Portable["Portable Work Module — transferable"]
        J["Jarvis Engine\nobserve · initiate · route · reason · coordinate"]
        K["Deterministic policy and authority kernel"]
        W["Work event ledger · plans · receipts · snapshots"]
        R["Signed runtime · contracts · capability registry"]
        J --> K
        R --> J
        W <--> J
    end

    subgraph Operational["Operational context compartments"]
        E["Email"]
        C["Calendar"]
        S["Messages and social"]
        P["Project and platform state"]
    end

    subgraph Private["Personal Vault — authorised laptops only"]
        X["Immutable exports"]
        D["Operator memory-directive ledger"]
        M["Validated memory"]
        PPP["Canonical Personal Professional Profile"]
        B["Behaviour · preferences · relationships"]
        T["Promoted local twin artefacts"]
        G["Local context and inference broker"]
        X --> M
        D --> M
        M --> G
        PPP --> G
        B --> G
        T --> G
    end

    Operational --> J
    G -->|"mounted in Twin Mode"| J

    K --> A["Typed domain adapters"]
    A --> ORC["Orchestrator-v3\ninternal Overseer"]
    A --> MA["Market Aligner\ninternal JAA + career profiler"]
    A --> DS["Dubbing Studio"]
    A --> OT["Other digital infrastructure"]

    ORC -->|"certificates · events · outcomes"| W
    MA -->|"application events · outcomes"| W
    DS -->|"receipts · outputs"| W
    OT -->|"receipts · outcomes"| W
    MA -->|"PPP delta proposals"| PPP
```

[Open the detailed end-state operational flow](docs/11_JARVIS_SOVEREIGN_RUNTIME_PROPOSED_ARCHITECTURE.md#31-end-state-operational-flow).

## Scope and authority

This repository contains explanations and planning material only. It contains no runtime
implementation, Personal Vault contents, credentials, work state, deployment activation, or grant
of authority. The documents retain their own proposal and ratification status.

## Canonical source identities

- Proposed architecture SHA-256: `fc65e8221416178eb3c7ef2801b4e631269853926a648683648dc7cc293dc245`
- Implementation blueprint SHA-256: `eebb08490dcba120f793f4a5c513a71accefa0d4871c2b05f24d8d1d6a3ae6a8`
