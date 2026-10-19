# 🍕 1337 Pizza

[![CI](https://github.com/elyesa-a/Pizza_Shop/actions/workflows/ci.yml/badge.svg)](https://github.com/elyesa-a/Pizza_Shop/actions/workflows/ci.yml)

REST-Backend und Web-Frontend für einen fiktiven Pizza-Lieferdienst "für Nerds". Teamprojekt aus dem Modul *Software Engineering* an der Hochschule Darmstadt, entwickelt nach agilen Methoden mit User Stories, Code Reviews und einer vollständigen CI/CD-Pipeline.

## Funktionen

- **Speisekarte verwalten:** Pizzasorten, Teige, Saucen, Beläge und Getränke anlegen, ändern und löschen
- **Bestellungen:** Bestellungen mit Pizzen und Getränken anlegen, Preis berechnen, Status verfolgen (offen → in Zubereitung → in Lieferung → abgeschlossen) und nach Status filtern
- **Lagerbestand:** Zutaten und Getränke werden bei jeder Bestellung automatisch abgebucht, nicht verfügbare Pizzen lassen sich nicht bestellen
- **Nutzerverwaltung** und ein **Web-Frontend** zum Bestellen und Verwalten

## Technik

| Bereich | Umsetzung |
|---|---|
| Backend | Python, **FastAPI**, Pydantic, SQLAlchemy 2 |
| Datenbank | **PostgreSQL**, Migrationen mit **Alembic** |
| Frontend | **Preact** mit **TypeScript**, Flux-Architektur, Ports & Adapters, API-Client aus OpenAPI generiert |
| Tests | **pytest** (Unit- und Integrationstests), **Tavern** (API-Servicetests), **Vitest** (Frontend) |
| Qualität | Ruff, SonarQube, Coverage-Reports |
| Infrastruktur | **Docker**, Kubernetes (Staging und Produktion), GitLab CI/CD mit Smoke-Tests, GitHub Actions |

## Starten

Voraussetzung: [Docker](https://docs.docker.com/get-docker/)

```bash
docker compose up -d --build
docker compose exec web sh -c "PYTHONPATH=. alembic upgrade head"
docker compose exec web sh -c "PYTHONPATH=. uvicorn app.main:app --host 0.0.0.0"
```

Die interaktive API-Dokumentation (Swagger) ist dann unter http://localhost:8000/docs erreichbar.

### Tests ausführen

```bash
# Backend (braucht eine laufende PostgreSQL-Datenbank)
uv sync
uv run pytest tests/unit tests/integration

# Frontend
cd frontend && npm ci && npx vitest run
```

## Projektstruktur

```
├── app/
│   ├── api/v1/endpoints/   # Router, CRUD-Logik und Schemas je Ressource
│   ├── database/           # SQLAlchemy-Modelle und Alembic-Migrationen
│   └── main.py             # FastAPI-Anwendung
├── frontend/               # Preact-Frontend (TypeScript)
├── tests/
│   ├── unit/               # Schema-Tests
│   ├── integration/        # CRUD-Tests gegen echte Datenbank
│   └── service/            # API-Tests mit Tavern
├── infra/                  # Dockerfiles und Kubernetes-Deployments
├── doc/                    # Domänenmodell, CI/CD-Strategie, Konventionen
└── .gitlab-ci.yml          # Original-Pipeline (Build, Tests, Deployment auf Kubernetes)
```

Mehr Details in der [Dokumentation](doc/README.md).

## Team und mein Beitrag

Entstanden im Team mit vier Studierenden. Ich habe vor allem an der **Qualitätssicherung** gearbeitet: Integrationstests für Bestellungen, Beläge und Nutzer (Anlegen, Lesen, Löschen gegen eine echte Datenbank), Anpassungen an der Lagerlogik und die Dokumentation der Tooling-Richtlinien (Ruff).
