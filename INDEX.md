# Documentation Package Index

## Repository Structure

```text
dipladeurs-gis-platform/
├─ README.md
├─ .gitignore
├─ .env.example
├─ docs/                          ← all project documentation
├─ frontend/                      ← Leaflet-based web client
├─ backend/                       ← REST API (src/ and tests/)
├─ database/                      ← migrations, seeds, schema
├─ geoserver/                     ← SLD styles and setup guide
├─ scripts/                       ← GIS processing, loaders, maintenance
├─ infra/                         ← nginx and Docker configuration
├─ sample-data/                   ← demo datasets and test fixtures
└─ .claude/                       ← Claude AI assistant configuration
```

## Documentation Map

All project documents live under `docs/`, grouped by concern.

### docs/product/

Documents defining what the platform does and for whom.

| Document | Description |
|----------|-------------|
| VISION_PRODUCT.md | Product vision, scope, time horizons, and assumptions |
| FUNCTIONAL_REQUIREMENTS.md | Detailed functional requirements for the platform |
| ROLES_AND_PERMISSIONS.md | Role hierarchy, permission matrix, ownership rules |
| BACKLOG_MVP.md | Work packages and delivery order for the MVP |
| BRANDING_AND_UI_GUIDELINES.md | Visual identity, colour palette, layout, and UI tone |

### docs/architecture/

Documents defining how the platform is built and deployed.

| Document | Description |
|----------|-------------|
| SYSTEM_ARCHITECTURE.md | Target architecture, components, data flows, multi-instance model |
| API_CONTRACTS.md | API endpoint catalogue and payload contracts |
| DEVELOPER_ONBOARDING.md | Orientation for new developers, repo structure, working rules |
| LOCAL_ROLLOUT_PLAN.md | Deployment sequence, server baseline, smoke tests, go-live checklist |

### docs/data/

Documents defining the data structures and governance.

| Document | Description |
|----------|-------------|
| DATA_MODEL.md | Conceptual and logical data model, entity definitions, relationships |
| LAYER_CATALOG.md | Registry of institutional and uploaded layers |
| AUDIT_AND_TRACEABILITY.md | Audit event schema, triggers, retention, restoration |

### docs/gis/

Documents defining GIS-specific standards and practices.

| Document | Description |
|----------|-------------|
| GIS_STANDARDS.md | CRS policy, format support, metadata requirements, quality checklist |

### docs/testing/

Documents defining validation and acceptance processes.

| Document | Description |
|----------|-------------|
| UAT_TESTING_PLAN.md | UAT scenarios, acceptance criteria, test execution plan |

### docs/decisions/

Architecture Decision Records (ADRs) are added here as real decisions are documented during development. ADRs follow sequential numbering with descriptive names.

Example format: `ADR-001-short-description.md`

No placeholder ADRs should be created. Each ADR should record a specific decision that was actually made, including context, options considered, and rationale.

## Notes

- All document content is written in English.
- Filenames use English names aligned with repo conventions.
- The documentation package is aligned to the current vision, the continuity document, and the existing beta viewer.
- Documents are living references — they should be updated as implementation decisions are made.
