<!-- 3/A) Swimlane – ONLINE E2E (valós idejű beküldés/validáció/ack) -->
```
  flowchart LR
  ## 3/A) Swimlane – ONLINE E2E (valós idejű beküldés/validáció/ack)
  %% ---- Styles (optional) ----
  classDef lane fill:#f6f8fa,stroke:#c9d1d9,color:#24292f;
  classDef decision fill:#fff3cd,stroke:#d39e00,color:#533f03;
  classDef system fill:#e8f0fe,stroke:#4c8bf5,color:#1a3d8f;
  classDef store fill:#eaffea,stroke:#2da44e,color:#0f3d1b;
  classDef ext fill:#f0f0ff,stroke:#6f42c1,color:#2f1c6b;
  classDef error fill:#ffeef0,stroke:#cf222e,color:#82071e;

  %% ---- Lanes ----
  subgraph L1[Üzlet / Compliance]
    A1[Trigger: bejelentési kötelezettség esemény<br/>(pl. ügylet/ügyfél/riasztás)]:::lane
    A2[Szabályok/megfelelőség döntés<br/>(PMT/NAV típus, határidő, kötelező mezők)]:::lane
  end

  subgraph L2[Csatorna / Forrás (API/UI/Backoffice)]
    B1[Kérés összeállítása + korrelációs azonosító]:::lane
    B2[Kérés küldése NEW_SYSTEM felé]:::lane
  end

  subgraph L3[NEW_SYSTEM (Rendszer2)]
    C1[Online intake API]:::system
    C2{XSD verzió kiválasztás<br/>(PMT_17..PMT_25)}:::decision
    C3[XSD validáció + üzleti validáció]:::lane
    C4{Valid?}:::decision
    C5[Perzisztálás (csak NEW write)]:::system
    C6[(NEW DB)]:::store
    C7[Üzenet/payload előállítás NAV felé]:::lane
    C8[Aszinkron küldés / retry / idempotencia]:::lane
  end

  subgraph L4[OLD_SYSTEM (Rendszer1) - Read-only]
    D1[Read-only lookup (opcionális)<br/>történeti adatok / referencia]:::system
    D2[(OLD DB)]:::store
  end

  subgraph L5[DW / Riport]
    E1[Streaming/CDC/ETL: NEW -> DW]:::lane
    E2[(DW / Lakehouse)]:::store
    E3[Riportok / Audit lekérdezések]:::lane
  end

  subgraph L6[NAV (külső)]
    F1[NAV fogadó endpoint]:::ext
    F2[Visszaigazolás / hiba (ACK/NACK)]:::ext
  end

  subgraph L7[Monitoring/Audit]
    G1[Audit log / trace / naplózás]:::lane
    G2[Monitoring / alerting]:::lane
  end

  %% ---- Flow ----
  A1 --> A2 --> B1 --> B2 --> C1 --> C2 --> C3 --> C4
  C4 -- "Igen" --> C5 --> C6 --> C7 --> C8 --> F1 --> F2 --> G1 --> G2
  C4 -- "Nem" --> X1[Hiba: validációs hiba lista<br/>vissza a forráshoz / javítás]:::error --> B1

  %% ---- Optional OLD lookup ----
  C1 -. "opcionális lookup" .-> D1 --> D2 -. "referencia adatok" .-> C3

  %% ---- DW feed ----
  C5 --> E1 --> E2 --> E3
  G1 --> E3
```
<!--
https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams
your comment goes here
and here
-->
