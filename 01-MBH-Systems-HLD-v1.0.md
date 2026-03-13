###### 01-MBH-Systems-HLD-v1.0.md

Normalizálva elkészítve feldolgozáshoz, de minden összevont elem mögött megőrizve a forrás-elemek listáját (zárójelben), hogy vissza tudd követni az Excelhez.
A dokumentációt chatben, sima markdownban fogom generálni, úgy, hogy később egy az egyben bedobhasd GitHubra.
A felépítést következetesen megtartom:

- High-Level Architecture Overview (HLD v1)
- Normalizált kategória-fastruktúra (HLD v2)
- Funkcionális domainenkénti mapping (HLD v3)
- Kapcsolati architektúra (HLD v4)
- Deep-dive LLD rétegenként (LLD sorozat)
- PlantUML / Mermaid diagramok (végső lépés)

És minden szintnél:

- ✔ normalizált komponensnevek
- ✔ összevont rendszerek (pl. “UFO platform stack”)
- ✔ zárójelben a teljes forrás névsor, 1×, tisztán, duplikációmentesen
- ✔ ne legyen redundancia, de legyen visszakövethetőség

## 🟦 HLD v1 — MBH High-Level Architecture Overview 🚀
Egységes, rétegzett áttekintés az MBH teljes rendszerlandscape-jéről (összes forráselem normalizált + összevont áttekintése az Excel alapján)
### 1. A teljes MBH IT-architektúra rétegei
Az MBH jelenlegi rendszerlandscape-je rétegelt architektúrára épül, ahol a fő rétegek:
#### 1. CHANNEL LAYER (ügyfélcsatornák)
A felhasználók által közvetlenül elért rendszerek:
- Mobil alkalmazások
(MBH Next Android, MBH Next iOS, MBH MobilApp (OneApp), Takarék MobilApp, MobilBank, Mobilbróker…)
- Webes csatornák
(Netbank, Újgenerációs Netbank, Web Customer Front End, MBH Next Web, Takarék Netbank…)
- Fióki és ügyintézői csatornák
(Fióki Frontend, Branch Teller, Contact Center felületek, Ügyfélhívó/Q system…)
- Speciális ügyfélportálok
(Takarék Electra ügyfélprogram, MBH Vállalati Mobilapp, Vállalati Netbank, eKapu/Ügyfélkapu integrációk…)

Ez a réteg minden esetben backend service-ekhez kapcsolódik.

#### 2. SERVICE / BACKEND LAYER
A csatornákat kiszolgáló üzleti logika döntő része:
- MBH Next Channel Services
(Android/iOS/Web Channel Services, Mobile Shared Services, Shared Channel Services)
- Üzleti modulok
(Oasis, IRM scoring, Origination, C360 backend, Ügyfélkezelés modulok, AccountProfile, Számlanyitás modul…)
- Tranzakciókezelés
(Payment Engine, GiroConnector, Transaction-service, Pénzügyi tranzakció modul…)
- Workflow & document rendszerek backendje
(UFO Engine backend elemei, WFC workflow-k, BMS workflows stb.)

Ez a réteg az üzleti működés magja.

#### 3. INTEGRATION LAYER
A rendszerek közti kommunikáció backbone-ja:
- ESB / MQ / JMS központi integrációs komponensek
(ESB, ESBLog, File2MQ, jmsGateway, IDA, Integration HUB…)
- API Gateway & OpenAPI layer
(API Gateway, OKM modul, Public Adapter, Open API…)
- File alapú integrációk
(FileTransfer, CafSFTP, ConnectDirect, Giro file rendszerek…)
- Külső partnerekhez kapcsolódó adapternode-ok
(ING, MFB, MNB, Központi adatforgalmazó ADF, GIRO, VISA/MasterCard modulok…)

Ez a réteg felel a stabil, skálázható kapcsolatokért a külső és belső rendszerek között.


