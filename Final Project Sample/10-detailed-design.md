# 10. Detailed Design

## 10.1 Design Patterns Applied

| Pattern | Category | Where Applied | Problem Solved |
|---------|----------|---------------|----------------|
| Repository | Structural | Data access layer | Abstracts database operations behind a consistent interface, making it easy to switch databases or add caching. |
| MVC | Architectural | Frontend (React) + Backend (Express) | Separates UI, business logic, and data concerns. Controllers handle requests, services contain logic, views render UI. |
| Observer | Behavioral | Notification system | When attendance is saved or a grade is recorded, the notification service is triggered automatically without tight coupling. |
| Factory | Creational | Report generation | A ReportFactory creates the correct report type (attendance report, grade report, report card) based on input parameters. |
| Strategy | Behavioral | Grade calculation | Different grading strategies (weighted average, points-based, curve) can be swapped without changing the calling code. |

## 10.2 SOLID Principles in Practice

**Single Responsibility (SRP):**
Each service handles one domain area. `AttendanceService` only manages attendance logic. `GradeCalculationService` only calculates grades. Neither service knows about the other's internals.

```
// Good: Each class has one reason to change
AttendanceService   → manages attendance records
NotificationService → sends emails
GradeService        → handles grade CRUD
GradeCalculator     → computes averages
```

**Open/Closed (OCP):**
The grade calculation supports new strategies without modifying existing code. Adding a "curve" grading strategy means creating a new class, not editing the existing weighted-average calculator.

```
interface IGradeStrategy {
    calculate(grades: Grade[], weights: CategoryWeight[]): number
}

class WeightedAverageStrategy implements IGradeStrategy { ... }
class PointsBasedStrategy implements IGradeStrategy { ... }
// New strategy added without touching existing code:
class CurveStrategy implements IGradeStrategy { ... }
```

**Dependency Inversion (DIP):**
Controllers depend on service interfaces, not concrete implementations. This allows unit testing with mock services.

```
// Controller depends on abstraction
class GradeController {
    constructor(private gradeService: IGradeService) {}
}

// In production: new GradeController(new GradeService(db))
// In testing:    new GradeController(new MockGradeService())
```

## 10.3 API Design (Sample Endpoints)

All endpoints follow RESTful conventions. Authentication is required via JWT Bearer token in the `Authorization` header.

### POST /api/submissions

**Purpose:** Student submits an assignment (US-010)

| Field | Value |
|-------|-------|
| Method | POST |
| URL | /api/submissions |
| Auth | Bearer token (Student role) |
| Content-Type | multipart/form-data |

Request:
```
{
  "assignmentId": 42,
  "file": <uploaded file>
}
```

Success Response (201 Created):
```json
{
  "submissionId": 187,
  "assignmentId": 42,
  "studentId": 15,
  "fileName": "homework3.pdf",
  "submittedAt": "2026-02-09T14:30:00Z",
  "isLate": false
}
```

Error Responses:
| Status | Condition | Body |
|--------|-----------|------|
| 400 | Invalid file type | `{ "error": "Only PDF, DOCX, ZIP files accepted" }` |
| 400 | File too large | `{ "error": "File must be under 10 MB" }` |
| 403 | Not enrolled | `{ "error": "You are not enrolled in this course" }` |
| 409 | Deadline passed | `{ "error": "Submission deadline has passed" }` |

---

### GET /api/courses/{courseId}/grades

**Purpose:** Student views grades for a course (US-013)

| Field | Value |
|-------|-------|
| Method | GET |
| URL | /api/courses/{courseId}/grades |
| Auth | Bearer token (Student or Parent role) |

Success Response (200 OK):
```json
{
  "courseId": 5,
  "courseName": "Mathematics 101",
  "studentId": 15,
  "grades": [
    {
      "assignmentTitle": "Homework 1",
      "category": "Homework",
      "score": 85.00,
      "maxScore": 100.00,
      "gradedAt": "2026-02-01T10:00:00Z"
    },
    {
      "assignmentTitle": "Quiz 1",
      "category": "Quiz",
      "score": 92.00,
      "maxScore": 100.00,
      "gradedAt": "2026-02-05T11:30:00Z"
    }
  ],
  "categoryWeights": [
    { "category": "Homework", "weight": 0.30 },
    { "category": "Quiz", "weight": 0.20 },
    { "category": "Exam", "weight": 0.50 }
  ],
  "courseAverage": 87.10,
  "letterGrade": "B+"
}
```

---

### POST /api/classes/{classId}/attendance

**Purpose:** Teacher records attendance (US-007)

| Field | Value |
|-------|-------|
| Method | POST |
| URL | /api/classes/{classId}/attendance |
| Auth | Bearer token (Teacher role) |

Request:
```json
{
  "date": "2026-02-09",
  "records": [
    { "studentId": 15, "status": "PRESENT" },
    { "studentId": 22, "status": "ABSENT" },
    { "studentId": 31, "status": "LATE" }
  ]
}
```

Success Response (201 Created):
```json
{
  "classId": 3,
  "date": "2026-02-09",
  "totalStudents": 28,
  "present": 25,
  "absent": 2,
  "late": 1,
  "notificationsQueued": 2
}
```

## 10.4 Error Handling Strategy

| Layer | Responsibility | Example |
|-------|---------------|---------|
| Controller | Catch errors, return appropriate HTTP status | 400 for validation, 404 for not found, 500 for server error |
| Service | Throw domain-specific errors | `DeadlinePassedError`, `CourseFullError` |
| Repository | Throw data access errors | `RecordNotFoundError`, `DuplicateEntryError` |
| Middleware | Global error handler, logging | Log all 500 errors; never expose stack traces to client |

---

[← Previous: Architecture](./09-architecture.md) | [Back to Index](./00-index.md) | [Next: UI/UX Design →](./11-ui-ux-design.md)
