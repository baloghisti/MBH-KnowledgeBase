### SAD-v0.1-SEQUENCE-Diagram-READ.md
```mermaid
sequenceDiagram
  autonumber
  participant PFE as PFE / Mitra (Posta Frontend)
  participant CC3 as CC3 (FFE frontend)
  participant OCS as OCS (OmniChannel Services)
  participant C360 as C360 (read model)
  participant FLEX as Flexcube (system of record)

  Note over PFE: Constraint: csak postai szamla, nincs ugyfel-szintu lekerdezes, nincs derived history
  PFE->>OCS: KPKNY lekerdezes / megjelenites (csatorna hivas)
  CC3->>OCS: ID2514_17 - KPKNY lekerdezes
  OCS->>C360: KPKNY adatok lekerese (per szamla)
  C360-->>OCS: KPKNY adatok (aktualis rekord)
  OCS-->>PFE: Megjelenitesi adatok (statusz + datumok)
  OCS-->>CC3: Megjelenitesi adatok

  Note over FLEX,C360: C360 frissules: DG notify + arrangement-daily/init (eventual consistency)
  FLEX-->>C360: CREATEARRANGEMENTNOTIFY_DG / MODIFYARRANGEMENTNOTIFY_DG (DG notify)
  FLEX-->>C360: arrangement-daily / init (FIA batch)
