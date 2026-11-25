# Frontend Developer Guide — SMRS

**Audience:** Frontend developer  
**Frontend stack:** HTML, CSS, JavaScript  
**Status:** Draft 0.1 — API contracts are not finalized yet.

## 1. Purpose
This guide defines how the frontend should be organized and how it will communicate with the FastAPI backend. The backend's Swagger/OpenAPI specification is the source of truth for implemented endpoints.

## 2. Suggested frontend structure
```text
frontend/
├── index.html
├── pages/
│   ├── login.html
│   ├── admin-dashboard.html
│   ├── teacher-dashboard.html
│   ├── student-dashboard.html
│   ├── students.html
│   ├── teachers.html
│   ├── subjects.html
│   └── results.html
├── css/
│   ├── style.css
│   └── responsive.css
└── js/
    ├── config.js
    ├── api.js
    ├── auth.js
    ├── components/
    └── pages/
```
The team may adjust this structure together before implementation.

## 3. API base URL
Keep the API base URL in one configuration location. Proposed local value:
```js
const API_BASE_URL = "http://127.0.0.1:8000/api/v1";
```
This is a placeholder until the backend route prefix and local ports are confirmed. Do not repeat the base URL in every page.

## 4. API communication
- Use `fetch()` or an agreed HTTP client.
- Send JSON with `Content-Type: application/json` when submitting JSON.
- Attach the agreed authentication credential to protected requests.
- Handle loading, success, empty, validation, unauthorized, forbidden, and server-error states.
- Do not assume an endpoint exists until it appears in the backend OpenAPI/Swagger docs.

Example request pattern (illustrative only):
```js
async function getStudents() {
  const response = await fetch(`${API_BASE_URL}/students/`, {
    headers: {
      // Add the agreed Authorization header or use credentials: "include"
    }
  });

  if (!response.ok) {
    throw new Error(`Request failed: ${response.status}`);
  }

  return response.json();
}
```

## 5. Proposed API areas
These are resource areas, not confirmed endpoint paths:
- Authentication: login, current user, logout.
- Institutions: institution details and management.
- Students: list, create, view, update, deactivate.
- Teachers: list, create, view, update.
- Classes/programmes: academic groups and enrolments.
- Subjects: subject management.
- Assignments: teacher-to-class/subject assignments.
- Examinations: exams and components.
- Results: direct marks and component marks.
- Reports: Excel and PDF exports.

The backend developer must provide the final endpoint table, request examples, response examples, and error formats after implementation.

## 6. Results screen expectations
The results page should support:
- Selecting academic year/period.
- Selecting class or programme.
- Selecting subject.
- Selecting examination or direct-entry assessment.
- Displaying student marks and grades.
- Showing validation messages for invalid marks.
- Showing draft/submitted/published status when supported by the API.
- Exporting only the currently selected, authorized dataset.

## 7. Downloading Excel/PDF
The backend should return a file response. The frontend should read it as a `Blob`, then trigger a download. Confirm the actual response headers and filename with the backend developer.

Illustrative pattern:
```js
async function downloadReport(url, filename) {
  const response = await fetch(url, {
    // Include the agreed authentication method.
  });

  if (!response.ok) {
    throw new Error(`Download failed: ${response.status}`);
  }

  const blob = await response.blob();
  const objectUrl = URL.createObjectURL(blob);
  const link = document.createElement("a");
  link.href = objectUrl;
  link.download = filename;
  link.click();
  URL.revokeObjectURL(objectUrl);
}
```

## 8. Security rules
- Never connect directly to PostgreSQL from browser JavaScript.
- Never place secrets or database credentials in frontend files.
- Do not trust role names or institution IDs supplied by the browser.
- Backend authorization is mandatory for every protected operation.
- Do not store passwords.
- Agree with the backend developer on cookie/token handling before implementing login.

## 9. Error handling
The backend team will publish the actual error response contract. Until then, the frontend should handle standard HTTP statuses:
- `400`: invalid request/business rule.
- `401`: missing or invalid authentication.
- `403`: authenticated but not permitted.
- `404`: resource not found.
- `422`: validation error (common in FastAPI).
- `500`: unexpected server error.

Do not hard-code assumptions about the exact JSON error shape until it is documented.

## 10. Definition of done for a frontend issue
- Page works at the agreed browser sizes.
- Form validation and error states are implemented.
- API integration uses the documented contract.
- Loading and empty states are handled.
- No secrets are committed.
- Changes are pushed to a feature branch and submitted as a Pull Request.
