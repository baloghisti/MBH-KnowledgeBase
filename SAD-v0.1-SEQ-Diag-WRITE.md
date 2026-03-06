### SAD-v0.1-SEQUENCE-Diagram-WRITE.md
```mermaid
sequenceDiagram
  autonumber
  participant CC3 as CC3 (FFE frontend)
  participant OCS as OCS (OmniChannel Services)
  participant FA as Flexadapter
  participant FLEX as Flexcube (system of record)

  CC3->>OCS: ID2514_15 - KPKNY letrehozas
  OCS->>FA: ID2514_18 - letrehozas tovabbitas
  FA->>FLEX: ID2514_20 - core letrehozas (forgatas TYPE/mezok szerint)
  FLEX-->>FA: valasz / statusz
  FA-->>OCS: valasz / statusz
  OCS-->>CC3: valasz / statusz

  CC3->>OCS: ID2514_16 - KPKNY visszavonas
  OCS->>FA: ID2514_19 - visszavonas tovabbitas
  FA->>FLEX: ID2514_21 - core visszavonas (forgatas ok szerint)
  FLEX-->>FA: valasz / statusz
  FA-->>OCS: valasz / statusz
  OCS-->>CC3: valasz / statusz
