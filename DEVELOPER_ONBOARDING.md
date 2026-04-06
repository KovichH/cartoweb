# Developer Onboarding

## 1. Purpose

This document helps new developers or technical collaborators understand the DIPLADEURS GIS platform project quickly and work safely on it.

## 2. Project Summary

This is an internal web GIS platform for the Municipality of Tonalá, focused on planning, environmental analysis, project tracking, and technical review. The current beta proved the interface concept in a single self-contained HTML file, but production requires server-backed architecture.

## 3. What Exists Today

Current proven capabilities from the beta include:

- basemap switching,
- display of PMDU urban districts,
- loading of GeoJSON, SHP, KML, KMZ,
- automatic reprojection handling,
- polygon drawing,
- per-project data panel,
- tag-style fields,
- link attachment,
- map image export,
- export of enriched spatial data,
- basic layer styling.

## 4. What Must Change for Production

The prototype is not the final architecture. Production work must move toward:

- frontend separated from data,
- API-backed persistence,
- PostGIS storage,
- GeoServer-managed layers,
- internal server deployment,
- auditable write operations.

Do not design long-term features assuming `file://` execution or embedded GeoJSON in the HTML.

## 5. Recommended Stack

- Frontend: Leaflet-based web client
- Reverse proxy/web server: nginx
- GIS services: GeoServer
- Spatial database: PostgreSQL + PostGIS
- Raster tiles: tiled outputs served locally
- OS: Ubuntu LTS on local server

## 6. Working Rules

- preserve real municipal workflow priorities,
- prefer incremental delivery,
- validate assumptions with existing project docs,
- avoid overengineering phase 1,
- protect auditability and data integrity,
- do not break the proven interaction model unless there is a strong reason.

## 7. Repository Structure

The repository follows a monorepo layout aligned with the system architecture. The structure is intentionally lean for phase 1 and should grow only as real implementation needs arise.

```text
dipladeurs-gis-platform/
├─ README.md
├─ .gitignore
├─ .env.example
├─ docs/
│  ├─ product/
│  │  ├─ VISION_PRODUCT.md
│  │  ├─ FUNCTIONAL_REQUIREMENTS.md
│  │  ├─ ROLES_AND_PERMISSIONS.md
│  │  ├─ BACKLOG_MVP.md
│  │  └─ BRANDING_AND_UI_GUIDELINES.md
│  ├─ architecture/
│  │  ├─ SYSTEM_ARCHITECTURE.md
│  │  ├─ API_CONTRACTS.md
│  │  ├─ DEVELOPER_ONBOARDING.md
│  │  └─ LOCAL_ROLLOUT_PLAN.md
│  ├─ data/
│  │  ├─ DATA_MODEL.md
│  │  ├─ LAYER_CATALOG.md
│  │  └─ AUDIT_AND_TRACEABILITY.md
│  ├─ gis/
│  │  └─ GIS_STANDARDS.md
│  ├─ testing/
│  │  └─ UAT_TESTING_PLAN.md
│  └─ decisions/
│     └─ (ADRs added as real decisions are made)
├─ frontend/
│  ├─ index.html
│  ├─ css/
│  ├─ js/
│  └─ assets/
├─ backend/
│  ├─ src/
│  └─ tests/
├─ database/
│  ├─ migrations/
│  ├─ seeds/
│  └─ schema/
├─ geoserver/
│  ├─ styles/
│  └─ setup.md
├─ scripts/
│  ├─ gis/
│  ├─ loaders/
│  └─ maintenance/
├─ infra/
│  ├─ nginx/
│  └─ docker/
├─ sample-data/
└─ .claude/
   └─ settings.json
```

### Structure rationale

- **docs/**: mirrors the existing documentation package, grouped by concern.
- **frontend/**: starts with the extracted prototype. Initially simple HTML/CSS/JS files; internal organisation grows with complexity.
- **backend/**: lightweight REST API. Module-level subdirectories are added when modules are implemented, not before.
- **database/**: migrations and seeds are version-controlled from the start.
- **geoserver/**: contains SLD style files and a setup guide for reproducible configuration. GeoServer's runtime data directory lives outside the repo (typically `/var/lib/geoserver/data`).
- **scripts/**: operational utilities for GIS processing, data loading, and maintenance.
- **infra/**: nginx configuration and Docker setup for the single internal server. Staging/production separation is handled via environment variables, not duplicated directory trees.
- **sample-data/**: demo datasets and test fixtures in a single directory.
- **decisions/**: Architecture Decision Records are created as real decisions are documented, not pre-populated with placeholders.

### Multi-instance readiness

The system architecture defines a future multi-instance model. When this becomes active, the structure can extend as follows:

```text
frontend/
├─ core/              ← shared viewer engine
└─ instances/
   └─ dipladeurs/     ← instance-specific config, branding, layer groups
```

This separation is deferred until a second instance is actually needed.

## 8. Coding Expectations

- clear naming,
- minimal hidden magic,
- explicit configuration,
- traceable database migrations,
- defensive validation for uploads,
- consistent error payloads,
- commit discipline aligned with feature scope.

## 9. GIS-Specific Expectations

- treat CRS handling as a first-class concern,
- do not silently trust uploaded geometries,
- prefer server-side normalisation,
- document any schema assumptions for layers,
- keep performance in mind for large datasets.

## 10. Backend Expectations

- every write path should consider audit logging,
- permissions must be enforced server-side,
- avoid direct destructive deletion unless intentionally designed,
- structure endpoints around business capabilities, not UI fragments alone.

## 11. Frontend Expectations

- keep the current usability strengths,
- preserve clarity of project panel interactions,
- maintain responsiveness for technical users,
- do not couple display logic to hardcoded embedded datasets,
- keep configuration flexible for future multi-instance use.

## 12. First Tasks for a New Developer

1. Read all project docs in `/docs`
2. Understand current beta behaviour
3. Review target architecture and data model
4. Set up local development environment
5. Validate a minimal flow: load map -> fetch layer -> create project -> persist -> retrieve
6. Review permissions and audit requirements before implementing edit features

## 13. Common Pitfalls

- assuming prototype storage patterns are acceptable for production,
- ignoring CRS edge cases,
- mixing temporary uploaded data with managed institutional layers,
- implementing permissions only in the UI,
- treating audit logging as optional,
- creating empty directories or scaffolding "for the future" without current need.

## 14. Definition of Done Guidance

A feature is not done unless:

- business behaviour works,
- errors are handled,
- permissions are enforced,
- audit impact is considered,
- documentation is updated where needed.

## 15. Recommended Additional Docs for Developers

- deployment runbook,
- environment variables reference,
- migration strategy,
- architecture decision records (ADRs),
- sample datasets for staging tests.
