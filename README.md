# Student Management & Result System (SMRS)

A multi-institution platform for managing students, teachers, academic structures, examinations, results, and downloadable reports.

## Project goals
- Support primary schools, secondary schools, colleges, and universities.
- Support direct marks entry and component-based examinations (CAT, midterm, final, etc.).
- Allow authorized teachers to view and export results for their assigned classes and subjects.
- Generate Excel and PDF reports.
- Keep each institution's data isolated from other institutions.

## Technology
| Area | Technology |
|---|---|
| Backend | Python, FastAPI |
| Frontend | HTML, CSS, JavaScript |
| Database | PostgreSQL |
| ORM / migrations | SQLAlchemy / Alembic |
| Authentication | JWT |
| Excel export | OpenPyXL |
| PDF export | ReportLab |
| API testing/docs | Swagger UI / Postman |

## Repository layout
```text
SMRS/
├── backend/
├── frontend/
├── docs/
├── .gitignore
└── README.md
```

## Documentation
- [System requirements](docs/SYSTEM_REQUIREMENTS.md)
- [System architecture](docs/SYSTEM_ARCHITECTURE.md)
- [Database design](docs/DATABASE_DESIGN.md)
- [Frontend developer guide](docs/FRONTEND_DEVELOPER_GUIDE.md)
- [GitHub workflow](docs/GITHUB_WORKFLOW.md)

## Development status
This repository is in the planning/design stage. API paths, request schemas, response schemas, and database constraints are proposals until implemented and verified in the backend's Swagger documentation.

## Local development (planned)
Backend and frontend setup instructions will be added after the initial project scaffolding is agreed by the team.
