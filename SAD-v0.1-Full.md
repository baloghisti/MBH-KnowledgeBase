### SAD v0.1 Test - Full
```mermaid
flowchart TB

subgraph FE[Frontend / Csatornak]
  PFE[PFE / Mitra]
  CC3["CC3 (FFE)"]
end

subgraph SRV[Szolgaltatasi es Integracios reteg]
  OCS[OCS]
  FA[Flexadapter]
end

subgraph CORE[Core es Read model]
  FLEX[Flexcube]
  C360[C360]
  BISZ[BISZ]
end

subgraph DOC[Dokumentum es Iktatas]
  DMWS[DMWS / DNXT]
  DMS[DMS One Ultimate]
  DARC[DARC]
end

NOTE{{PFE constraint: csak postai szamla; nincs ugyfel-szintu lekerdezes; nincs derived history}}
NOTE --- PFE

%% READ path
PFE -->|KPKNY lekerdezes megjelenites| OCS
CC3 -->|ID2514_17 query| OCS
OCS -->|KPKNY read| C360

%% WRITE path
CC3 -->|ID2514_15 create| OCS
CC3 -->|ID2514_16 cancel| OCS
OCS -->|ID2514_18 create| FA
OCS -->|ID2514_19 cancel| FA
FA -->|ID2514_20 core create| FLEX
FA -->|ID2514_21 core cancel| FLEX

%% Feeds Flexcube -> C360
FLEX -.->|CREATEARRANGEMENTNOTIFY_DG| C360
FLEX -.->|MODIFYARRANGEMENTNOTIFY_DG| C360
FLEX -.->|arrangement daily init FIA| C360

%% BISZ (koncepcionalis)
FLEX -.->|tovabbitas visszajelzes| BISZ

%% Posta FE legacy / existing
PFE -->|KUT_NOCOSTACC_DEAL MQ RQ RP| FLEX
PFE -->|TBD REST account detail KPKNY| C360

%% Account class (TBD)
OCS -->|TBD Account Class query| FLEX
FLEX -.->|TBD Account Class notify DG| C360

%% Document flows
OCS -->|generateDocument| DMWS
OCS -->|registerDocument| DMS
DMS -->|archivalas| DARC
