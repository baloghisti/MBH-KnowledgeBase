### SAD v0.1 Test - Simplefied
```mermaid
flowchart LR
  FE["Front-end (CC3 / PFE)"] -->|KPKNY lekerdezes| OCS[OCS]
  OCS -->|KPKNY adatok| C360[C360]

  CC3[CC3] -->|ID2514_15 create| OCS
  CC3 -->|ID2514_16 cancel| OCS
  OCS -->|ID2514_18 create| FA[Flexadapter]
  OCS -->|ID2514_19 cancel| FA
  FA -->|core update| FLEX[Flexcube]

  FLEX -.->|DG notify + daily init| C360

  NOTE{{PFE constraint: csak postai szamla, nincs ugyfel-szintu lekerdezes, nincs derived history}}
  NOTE --- FE
