## 1) BATCH Swimlane (E2E) – (GitHub-biztos, subgraph-os)

## ✅ Javítás 1: ugyanaz subgraph-pal, de GitHub-biztos node-szintaxissal
  > Figyelj rá, hogy:
  - minden elem külön sorban legyen,
  - subgraph lezárása csak end legyen,
  - az adatbázis node: NDB[(NEW DB)] (nem idézőjeles).
```mermaid
flowchart LR

subgraph L1
  L1T["Üzlet / Compliance"]
  A1["Trigger: bejelentési kötelezettség esemény"]
  A2["Megfelelőségi döntés: mit kell jelenteni (PMT/NAV)"]
end

subgraph L2
  L2T["Forrás (UI / API / Backoffice)"]
  B1["Kérés összeállítása + CorrelationId"]
  B2["Kérés küldése NEW_SYSTEM felé"]
end

subgraph L3
  L3T["NEW_SYSTEM (Rendszer2)"]
  C1["Online intake"]
  C2{"XSD verzió választás (PMT_17..PMT_25)"}
  C3["XSD + üzleti validáció"]
  C4{"Valid?"}
  C5["Persist (csak NEW ír)"]
  NDB["(NEW DB)"]
  C6["NAV payload előállítás"]
  C7["Küldés NAV felé (async/retry/idempotens)"]
end

subgraph L4
  L4T["OLD_SYSTEM (Rendszer1) - read-only"]
  O1["Opcionális read-only lookup (történeti / referencia)"]
  ODB["(OLD DB)"]
end

subgraph L5
  L5T["DW / Riport"]
  D1["NEW -> DW adatfolyam (ETL/stream)"]
  D2["(DW / Lakehouse)"]
  D3["Riportok + audit lekérdezések"]
end

subgraph L6
  L6T["NAV (külső)"]
  NAV1["NAV fogadó endpoint"]
  NAV2{"ACK / NACK"}
end

subgraph L7
  L7T["Monitoring / Audit"]
  M1["Audit log / trace"]
  M2["Monitoring / alert"]
end

A1 --> A2 --> B1 --> B2 --> C1 --> C2 --> C3 --> C4
C4 -- "Igen" --> C5 --> NDB --> C6 --> C7 --> NAV1 --> NAV2 --> M1 --> M2
C4 -- "Nem" --> E1["Hiba: validációs hibalista vissza a forráshoz"] --> B1

C1 -.-> O1
O1 --> ODB
ODB -.-> C3

C5 --> D1 --> D2 --> D3
M1 --> D3
```
## ✅ Javítás 2 (LEGbiztosabb): subgraph NÉLKÜL “swimlane-hatás” (ha a subgraph nálatok instabil)
Ha a GitHub Mermaid verziótok valamiért a subgraph-ot is érzékenyen kezeli, akkor ez a 100% kompatibilis fallback: nincs subgraph, csak “lane header” node-ok.
```mermaid
flowchart LR

L1["Üzlet / Compliance"] --> A1["Trigger esemény"] --> A2["Megfelelőségi döntés (PMT/NAV)"]

L2["Forrás (UI / API / Backoffice)"] --> B1["Kérés + CorrelationId"] --> B2["Kérés NEW_SYSTEM felé"]

L3["NEW_SYSTEM"] --> C1["Online intake"] --> C2{"XSD verzió (PMT_17..PMT_25)"} --> C3["Validáció (XSD + üzleti)"] --> C4{"Valid?"}
C4 -- "Igen" --> C5["Persist (csak NEW ír)"] --> NDB["(NEW DB)"] --> C6["NAV payload"] --> C7["Küldés NAV felé (retry)"] --> NAV1["NAV endpoint"] --> NAV2{"ACK/NACK"} --> M1["Audit"] --> M2["Monitoring"]
C4 -- "Nem" --> E1["Validációs hiba vissza"] --> B1

L4["OLD_SYSTEM (read-only)"] --> O1["Lookup (opcionális)"] --> ODB["(OLD DB)"]
C1 -.-> O1
ODB -.-> C3

L5["DW / Riport"] --> D1["ETL/stream"] --> D2["(DW)"] --> D3["Riportok"]
NDB --> D1
M1 --> D3
```
