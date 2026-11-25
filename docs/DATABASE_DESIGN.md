# Database Design — SMRS

**Status:** Draft 0.1 — conceptual model, not final SQL schema.

The exact columns, nullability, indexes, and constraints will be finalized before creating SQLAlchemy models and Alembic migrations.

## 1. Design principles
- Use PostgreSQL.
- Use primary keys for every table.
- Use foreign keys to preserve relationships.
- Use unique constraints for institution-scoped identifiers.
- Store timestamps for important records.
- Preserve historical enrolments and results.
- Keep institution data isolated.
- Avoid storing calculated values that can be derived unless there is a clear reporting/performance reason.

## 2. Proposed entities

| Entity | Purpose | Important relationships |
|---|---|---|
| `institutions` | Registered schools/colleges/universities | Has users, students, teachers, academic records |
| `users` | Login accounts | Belongs to institution where applicable; has role |
| `academic_years` | Academic year definitions | Belongs to institution |
| `academic_periods` | Terms/semesters | Belongs to academic year |
| `classes` | School classes or academic groups | Belongs to institution |
| `programmes` | College/university programmes (optional by institution type) | Belongs to institution |
| `students` | Student profile | Belongs to institution |
| `student_enrolments` | Student placement for an academic period | Links student to class/programme and period |
| `teachers` | Teacher profile | Belongs to institution |
| `subjects` | Subjects/courses | Belongs to institution |
| `class_subjects` | Subjects offered to a class/programme | Links academic group and subject |
| `teacher_assignments` | Teaching responsibility | Links teacher, class/programme, subject, and period |
| `examinations` | Exam/event definition | Belongs to institution and academic period |
| `exam_components` | CAT, midterm, final, practical, etc. | Belongs to examination |
| `results` | Student result record | Links student, subject, period/exam context |
| `result_components` | Marks for individual exam components | Links result and exam component |
| `grading_scales` | Configurable grading scheme | Belongs to institution |
| `grade_ranges` | Mark boundaries and grade labels | Belongs to grading scale |
| `audit_logs` | Records important actions | References actor and affected record |

## 3. Relationship overview
```text
Institution 1 ---- many Users
Institution 1 ---- many Students
Institution 1 ---- many Teachers
Institution 1 ---- many Classes
Institution 1 ---- many Subjects
Institution 1 ---- many Academic Years

Student 1 ---- many Student Enrolments
Class 1 ---- many Student Enrolments
Academic Period 1 ---- many Student Enrolments

Teacher 1 ---- many Teacher Assignments
Subject 1 ---- many Teacher Assignments
Class/Programme 1 ---- many Teacher Assignments

Examination 1 ---- many Exam Components
Student 1 ---- many Results
Subject 1 ---- many Results
Result 1 ---- many Result Components
Exam Component 1 ---- many Result Components

Grading Scale 1 ---- many Grade Ranges
```

## 4. Important modelling notes
- A student may move to a new class each academic year. Keep this history in `student_enrolments`.
- A teacher may teach several subjects/classes. Use `teacher_assignments` rather than a single subject field on the teacher.
- A subject may be taught to many classes and by many teachers. Model the assignment explicitly.
- A component-based result should identify the examination and its components.
- Direct marks entry should still have a clearly defined assessment context (for example, a final assessment record), so results remain traceable.
- Do not calculate a university GPA using school grading assumptions. Keep grading and GPA rules configurable.
- Add unique constraints only after deciding the correct scope, such as student registration number unique per institution.

## 5. Questions to resolve before SQLAlchemy implementation
1. Can a user have more than one role?
2. Can a teacher account also be a student account?
3. Do universities require departments, faculties, and programmes in the first release?
4. Are terms and semesters represented by one generic `academic_periods` table?
5. Can a student take the same subject more than once?
6. Can results be revised after publication? Is approval required?
7. Are component weights percentages, raw marks, or both?
8. Should ranking be per subject, class, examination, or all three?

## 6. Next deliverable
After these decisions, produce:
- Entity Relationship Diagram (ERD).
- Final table definitions with columns and data types.
- Foreign-key and unique constraints.
- SQLAlchemy model plan.
- Alembic migration plan.
