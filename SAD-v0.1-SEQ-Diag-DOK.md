### SAD-v0.1-SEQUENCE-Diagram-DOK.md
```mermaid
sequenceDiagram
  autonumber
  participant CC3 as CC3 / PFE (frontend)
  participant OCS as OCS
  participant DMWS as DMWS (DNXT)
  participant DMS as DMS One Ultimate
  participant DARC as DARC

  CC3->>OCS: Dokumentum generalas igeny (nyilatkozat / visszavonas)
  OCS->>DMWS: generateDocument (sablon + szemelyesitesi adatok)
  DMWS-->>OCS: PDF link / eredmeny
  OCS->>DMS: registerDocument (iktatasi metadata)
  DMS-->>OCS: iktatas eredmeny
  DMS->>DARC: archiválás
  DARC-->>DMS: archiválás eredmeny
  OCS-->>CC3: dokumentum link + iktatas statusz
