# System Architecture — SMRS

**Status:** Draft 0.1

## 1. High-level architecture

```text
Browser
  |
  | HTTPS / JSON / file downloads
  v
Frontend (HTML + CSS + JavaScript)
  |
  | REST API requests + Bearer access token
  v
FastAPI application
  |
  +-- API routers
  +-- Authentication & authorization
  +-- Pydantic schemas
  +-- Service layer (business rules)
  +-- Data access / SQLAlchemy
  +-- Report generation (Excel / PDF)
  |
  v
PostgreSQL
```

## 2. Backend responsibilities
- Validate all incoming data.
- Authenticate users and authorize every protected operation.
- Enforce institution-level data isolation.
- Apply examination calculations and grading rules.
- Persist data using SQLAlchemy.
- Manage schema changes through Alembic.
- Generate Excel and PDF files after authorization.
- Return consistent HTTP status codes and response formats.

## 3. Frontend responsibilities
- Render pages and reusable UI components.
- Collect user input and perform basic client-side validation.
- Call backend endpoints; do not connect directly to PostgreSQL.
- Display loading, success, empty, and error states.
- Show actions allowed for the current role.
- Download files returned by the backend.
- Never treat frontend role checks as a replacement for backend authorization.

## 4. Proposed backend layers
```text
backend/
└── app/
    ├── main.py
    ├── api/
    │   └── v1/
    │       ├── router.py
    │       └── endpoints/
    ├── core/          # config, security, logging
    ├── db/            # session and base
    ├── models/        # SQLAlchemy models
    ├── schemas/       # Pydantic request/response schemas
    ├── services/      # business logic
    └── reports/       # Excel/PDF generation
```
This is a proposed structure, not a requirement to create every file immediately.

## 5. API conventions
- API prefix proposal: `/api/v1`.
- Use nouns for resources: `/students`, `/teachers`, `/results`.
- Use HTTP methods consistently: GET, POST, PATCH/PUT, DELETE.
- Use query parameters for filtering, sorting, and pagination.
- Return JSON for normal API responses.
- Return a file response for exports with a correct content type and filename.
- Use ISO 8601 timestamps and document the timezone policy.

## 6. Authentication proposal
Protected requests use:
```http
Authorization: Bearer <access_token>
```
The backend validates the token and loads the current user and institution scope. Token storage strategy must be agreed before frontend implementation. For a browser application, prefer secure, HttpOnly cookies where the deployment architecture supports them; if bearer tokens are used in JavaScript, document the security trade-offs and avoid long-lived tokens.

## 7. Multi-institution isolation
Every institution-owned record must be associated with an institution, directly or through a validated parent relationship. Every service/query must scope records to the authenticated user's permitted institution(s). Never rely on a client-supplied `institution_id` alone to grant access.

## 8. Export flow
1. Frontend sends a GET request with selected filters.
2. Backend authenticates and authorizes the user.
3. Backend validates filters and retrieves only permitted records.
4. Report service creates the Excel/PDF file.
5. API returns a downloadable file with the appropriate content type and `Content-Disposition`.
6. Frontend handles the blob response and triggers download.

## 9. Development environments
- Local frontend and backend run separately during development.
- Configure the frontend API base URL through a configuration file or environment-specific setting.
- Configure CORS in FastAPI for the exact frontend development origin(s).
- Do not commit `.env`, API secrets, passwords, or production credentials.
