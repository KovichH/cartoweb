# DIPLADEURS GIS Platform

Internal web GIS platform for the Dirección de Planeación y Desarrollo Urbano Sustentable, Municipality of Tonalá, Jalisco.

## What is this

A spatial operations platform for urban planning, environmental analysis, project tracking, and technical review. The system serves internal municipal staff with role-based access, auditable workflows, and centrally managed geographic datasets.

## Architecture

```text
[Browser / Leaflet Frontend]
          |
          v
[nginx Reverse Proxy]
          |
   ─────────────────────────
   |             |          |
   v             v          v
[REST API]  [GeoServer]  [Tile Service]
   |             |          |
   v             v          |
[PostgreSQL + PostGIS]      |
   |________________________|
```

## Repository Layout

```
docs/           Project documentation (product, architecture, data, GIS, testing, decisions)
frontend/       Leaflet-based web client
backend/        REST API
database/       Migrations, seeds, and schema definitions
geoserver/      SLD styles and configuration guide
scripts/        GIS processing, data loaders, and maintenance utilities
infra/          nginx and Docker configuration
sample-data/    Demo datasets and test fixtures
```

## Getting Started

See `docs/architecture/DEVELOPER_ONBOARDING.md` for setup instructions and working rules.

## Technology Stack

- Frontend: HTML/CSS/JavaScript with Leaflet
- Web server: nginx
- API: lightweight REST backend
- GIS services: GeoServer
- Database: PostgreSQL + PostGIS
- Raster tiling: gdal2tiles-compatible workflow
- OS: Ubuntu LTS on municipal infrastructure

## Current Status

The platform has a proven HTML prototype (`visor_dipladeurs_v4.html`) demonstrating the target user experience. Production development follows the MVP backlog documented in `docs/product/BACKLOG_MVP.md`.

## Documentation

Full documentation index: `docs/INDEX.md`

## Internal Use Only

This platform is deployed on the municipal LAN and is not intended for public access.
