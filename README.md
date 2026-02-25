# MBH-KnowledgeBase
# MBH-WORKs
--------------------------
# Knowledge Base
Személyes szakmai tudásbázis:
- architektúra
- cloud
- devops
- jegyzetek

## Elvek
- Markdown first
- Simple > Clever
- Document decisions, not only solutions

## Használat
- Tartalom: `docs/`
- Lokálisan: `mkdocs serve`
- Publikálás: GitHub Pages
--------------------------

Elvárt struktúra (hasonló):
knowledge-base/
├─ README.md                 # mi ez a repo, hogyan használd
├─ mkdocs.yml                # navigáció, theme
├─ docs/
│  ├─ index.md               # Home / About
│  ├─ architecture/
│  │  ├─ index.md
│  │  ├─ event-driven.md     # ← MOST ERRE FÓKUSZÁLUNK
│  │  ├─ cqrs.md
│  │  └─ microservices.md
│  ├─ cloud/
│  │  ├─ index.md
│  │  └─ azure/
│  │     └─ landing-zones.md
│  ├─ devops/
│  │  └─ cicd.md
│  └─ notes/
│     └─ inbox.md            # nyers gondolatok
