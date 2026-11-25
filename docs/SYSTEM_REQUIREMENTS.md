# System Requirements — SMRS

**Status:** Draft 0.1  
**Purpose:** Define the first agreed scope before implementation.

## 1. Overview
SMRS is a multi-institution student, academic, examination, and result management system. An institution manages its own users and academic records. Users must only access data allowed by their role and institution membership.

## 2. Supported institution types
- Primary schools
- Secondary schools
- Colleges
- Universities

The system must support configurable academic structures. For example, a school may use classes and terms, while a university may use programmes, academic years, and semesters.

## 3. User roles
| Role | Main responsibilities |
|---|---|
| `SUPER_ADMIN` | Create/manage institutions and platform-level settings |
| `INSTITUTION_ADMIN` | Manage users, academic setup, students, teachers, subjects, and institution reports |
| `TEACHER` | View assigned classes/subjects, enter marks, view permitted results, export permitted reports |
| `STUDENT` | View own published results and download own report where enabled |

Role permissions must be enforced by the backend. Hiding a button in the frontend is not security.

## 4. Functional requirements

### 4.1 Authentication and access
- Users can sign in and sign out.
- Protected API operations require a valid access token.
- The backend identifies the current user and their role.
- Institution-scoped users cannot access another institution's records.
- Passwords must be securely hashed; plain-text passwords must never be stored.

### 4.2 Institution and academic setup
- Super admin can register and manage institutions.
- Institution admin can configure academic years and terms/semesters.
- The institution can configure its academic structure and grading rules.
- The system records which institution owns each institution-specific record.

### 4.3 Student management
- Authorized staff can create, view, update, search, and deactivate student records.
- A student belongs to an institution.
- Student enrolment/registration identifiers must be unique within the agreed scope.
- Student class placement must be tracked by academic year rather than overwriting historical class history.

### 4.4 Teacher and teaching assignments
- Authorized staff can create and manage teacher profiles.
- Teachers can be assigned to classes and subjects.
- A teacher can only enter or export marks for classes/subjects assigned to them, unless an administrator grants additional permission.

### 4.5 Subjects and classes
- Authorized staff can create and manage classes, programmes, and subjects as appropriate to the institution type.
- Subject/class assignments must be associated with the relevant academic period.

### 4.6 Examinations and results
The system supports both modes:

**Mode A — Direct marks**
- Authorized teacher enters a final mark for a student in a subject and academic period.

**Mode B — Component-based examination**
- An examination can contain components such as CAT 1, CAT 2, midterm, practical, and final.
- Each component has a maximum mark and a configured contribution/weight.
- The system validates marks against the component maximum.
- The final score is calculated using the institution's approved rules.

Rules to settle before implementation:
- Whether component scores are added directly or converted to weighted percentages.
- Whether missing components are treated as zero, incomplete, or excluded.
- Rounding precision.
- Whether a result can be edited after publication and who can approve changes.

### 4.7 Grading and ranking
- Grading scales are configurable by institution and, if required, academic level/programme.
- Grade boundaries must not be hard-coded globally.
- Pass/fail rules are configurable.
- GPA is optional and only enabled where the institution uses it.
- Class/subject ranking is optional and controlled by institution policy.
- Ranking must handle ties consistently.

### 4.8 Result publication and reports
- Results have a lifecycle such as `DRAFT`, `SUBMITTED`, `APPROVED`, and `PUBLISHED` (final states to be confirmed).
- Students can only see results that are published and belong to them.
- Teachers can view results within their assignment permissions.
- Authorized users can export filtered results to Excel and PDF.
- Exported files must respect the same authorization and institution boundaries as normal API requests.
- Reports should show institution, academic period, class/programme, subject, examination, student identifier, marks, grade, and generation date as applicable.

## 5. Non-functional requirements
- Security: password hashing, token validation, role checks, institution isolation, input validation.
- Data integrity: database constraints and transactions for result submissions.
- Performance: pagination and filtering for large student/result lists.
- Reliability: meaningful error responses and server-side logging.
- Maintainability: modular backend and reusable frontend components.
- Usability: responsive pages and clear validation/error messages.
- Auditability: record who created/updated/submitted/published marks and when.
- Backup: database backup and restore procedure before production use.

## 6. Out of scope for initial release
Unless separately approved:
- Online fee/payment processing.
- Attendance tracking.
- Parent/guardian portal.
- SMS/WhatsApp notifications.
- AI-based performance prediction.

## 7. Acceptance criteria for MVP
- Admin can configure an institution's academic year, classes, subjects, students, and teachers.
- Admin can assign teachers to classes and subjects.
- Authorized teacher can enter direct marks and component-based marks.
- Backend calculates results according to configured rules.
- Unauthorized users cannot access another institution's data or unassigned class results.
- Authorized teacher/admin can export filtered results to Excel and PDF.
- Student can view only their own published results.
- Core workflows are covered by API and integration tests.

## 8. Open decisions
Track decisions in GitHub Issues or a project decision log:
1. Institution hierarchy and university programme structure.
2. Academic period model: terms, semesters, or both.
3. Component weighting and missing-mark rules.
4. Grading/GPA configuration.
5. Result approval and publication workflow.
6. Whether students self-register or are created by administrators.
