# System Architecture

## 1. Architectural Position

The current browser-only HTML prototype demonstrated the target user experience, but it is not a production architecture. Production deployment requires a server-backed design because the platform must support:

- centrally persisted project data,
- multiple internal users,
- auditable workflows,
- large GIS volumes,
- controlled publication of layers.

## 2. Target Architecture Overview

The target architecture is a modular internal web GIS stack deployed on the municipal network.

```text
[Browser / Leaflet Frontend]
          |
          v
[Internal Web Server / Reverse Proxy]
          |
   -----------------------------
   |             |             |
   v             v             v
[App API]   [GeoServer]   [Tile Service]
   |             |             |
   v             v             |
[PostgreSQL + PostGIS]         |
   |___________________________|
```

## 3. Main Components

### 3.1 Frontend
A browser-based client used by internal staff. Main responsibilities:

- map rendering,
- layer controls,
- drawing and editing UI,
- project panel interaction,
- search and export initiation,
- consuming API and GIS services.

### 3.2 Reverse Proxy / Web Server
Recommended: nginx. Responsibilities:

- serve static frontend assets,
- route API requests,
- front GIS services under a controlled internal endpoint,
- improve security and maintainability.

### 3.3 Application API
A lightweight backend service providing business operations not covered by GeoServer alone. Responsibilities:

- authentication/session integration,
- project CRUD,
- audit registration,
- search endpoints,
- file ingestion orchestration,
- proxy integrations such as geocoding if adopted.

A lightweight REST API is sufficient for phase 1.

### 3.4 GeoServer
GeoServer is the central GIS service layer. Responsibilities:

- publish vector and raster layers,
- expose WMS/WFS/WMTS services,
- support styling and service-level access rules,
- decouple frontend from raw storage.

### 3.5 PostgreSQL + PostGIS
The core spatial database. Responsibilities:

- store project geometries,
- store uploaded vector datasets when promoted to managed data,
- store business tables and audit tables,
- support search and geospatial querying.

### 3.6 Raster Tile Service
Large imagery such as orthophotos should be converted to tiles and served efficiently through local infrastructure. Responsibilities:

- avoid direct browser loading of very large raster files,
- serve only visible tiles,
- reduce rendering latency.

## 4. Data Flow Patterns

### 4.1 Managed municipal layer
1. GIS manager prepares/publishes layer
2. Layer is stored in PostGIS or raster storage
3. GeoServer exposes the layer
4. Frontend consumes service endpoint

### 4.2 Uploaded user layer
1. User uploads supported file
2. API validates and normalises
3. Dataset is either stored temporarily or promoted to managed storage
4. Layer becomes visible in the frontend
5. Upload is logged

### 4.3 Project record update
1. User edits project geometry or business fields
2. Frontend sends update to API
3. API writes to PostGIS
4. API records audit entry
5. Updated state is returned to the client

## 5. Working CRS Strategy

A single internal working CRS should be defined for managed processing. All uploads should be normalised into the platform CRS, while service output can be adapted for web display.

Recommended principle:
- storage CRS defined institutionally,
- frontend display uses web-compatible CRS,
- transformations are handled server-side where possible.

## 6. Layer Organisation Model

The frontend information architecture should separate institutional managed layers by departmental area rather than mixing all visible layers into a single undifferentiated list.

Recommended top-level layer groups:
- General Directorate
- Building Control
- Ecology and Climate Change
- Urban Development
- Mobility and Planning
- Normativity
- Temporary User Uploads

This grouping should be driven by metadata stored in the layer catalogue, not by hard-coded frontend labels only.

## 7. Collaboration Model

Phase 1 collaboration is a shared workspace with central persistence, not full real-time conflict-free collaboration.

### Phase 1 rules
- all users view the same published datasets,
- edits are saved centrally,
- latest saved version becomes the current record,
- conflicting edits are mitigated operationally, not solved with advanced merge logic.

### Future phase
- optimistic locking,
- record-level edit locks,
- event-based sync,
- change conflict alerts.

## 8. Scalability Assumptions

The platform starts with approximately 12 concurrent users, but architecture should avoid dead ends.

Scalability expectations:
- support large layer volumes through GIS services,
- allow multiple departmental instances with shared core,
- separate configuration from application logic.

## 9. Multi-Instance Architecture

The recommended model is one shared engine with per-department configuration.

### Conceptual model

```text
visor-core (shared)        ← map engine, API, shared components
instances/dipladeurs       ← config, branding, layer groups, permissions
instances/other-department ← same structure, different config
services/geoserver         ← shared GIS service
services/postgis           ← shared database
```

### Repository mapping

In the monorepo, this translates to:

```text
frontend/
├─ core/              ← shared viewer engine (Leaflet, shared UI)
└─ instances/
   └─ dipladeurs/     ← instance config: layer groups, branding, field visibility

backend/
└─ src/
   └─ config/         ← per-instance configuration loaded at runtime
```

Phase 1 starts with a single instance (DIPLADEURS). The `core/instances/` split is deferred until a second instance is actually needed. The current flat `frontend/` structure is sufficient and should not be pre-split.

### Benefits
- avoids duplicated codebases,
- centralises maintenance,
- allows different visible layers, fields, and permissions per instance.

## 10. Security Boundaries

- Internal-only network deployment
- Authenticated access only
- Role enforcement in API and GIS publication layer
- Audit trail for business-critical mutations
- Limited destructive operations

## 11. Technology Recommendation

- Frontend: HTML/CSS/JavaScript with Leaflet
- Web server: nginx
- API: lightweight REST backend
- GIS services: GeoServer
- Database: PostgreSQL + PostGIS
- Raster tiling: gdal2tiles-compatible workflow
- OS: Ubuntu LTS on municipal infrastructure

## 12. Architectural Constraints

- No production dependence on embedded GeoJSON inside HTML
- No file:// execution model in production
- No assumption of public cloud dependency
- No heavy real-time collaboration framework in phase 1

## 13. Risks

- unclear data governance for incoming layers,
- lack of institutional metadata discipline,
- oversized raster sources without tiling,
- uncontrolled layer publication,
- editing conflicts if project ownership is undefined.

## 14. Recommended Next Architectural Deliverables

- deployment architecture diagram,
- environment configuration inventory,
- API endpoint catalogue,
- GIS publication standards,
- backup and recovery plan.
