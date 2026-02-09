# 12. Traceability Matrix

## 12.1 Purpose

The traceability matrix links every requirement to its downstream design artifacts, ensuring nothing is lost between analysis and design. Each row answers: where does this requirement appear in our use cases, classes, database tables, API endpoints, and UI screens?

## 12.2 Requirements Traceability

| Req ID | Requirement | Use Case | User Story | Domain Class | DB Table | API Endpoint | UI Screen |
|--------|-------------|----------|------------|-------------|----------|-------------|-----------|
| FR-001 | Manage user accounts | UC-001 | US-002 | User | users, students, teachers, parents | POST /api/users | Admin → Manage Users |
| FR-002 | Authenticate via email/password | UC-012 | US-001 | User | users | POST /api/auth/login | Login Page |
| FR-003 | Role-based access control | UC-012 | US-001 | User (Role enum) | users.role | Auth middleware | All dashboards |
| FR-006 | Create and manage courses | UC-002 | US-004 | Course | courses | POST /api/courses | Admin → Manage Courses |
| FR-008 | Enroll students in courses | UC-003 | US-005 | Enrollment | enrollments | POST /api/enrollments | Admin → Enrollments |
| FR-009 | Block enrollment when full | UC-003 | US-005 | Course.isFull() | courses.capacity | POST /api/enrollments (409) | Enrollment form error |
| FR-011 | Record daily attendance | UC-004 | US-007 | Attendance | attendance | POST /api/classes/{id}/attendance | Teacher → Attendance |
| FR-012 | Notify parents of absences | UC-004 (ext) | US-007 | — | — | Triggered internally | Email notification |
| FR-014 | Create assignments | UC-005 | US-009 | Assignment | assignments | POST /api/assignments | Teacher → Assignments |
| FR-015 | Submit assignments | UC-006 | US-010 | Submission | submissions | POST /api/submissions | Student → Submit |
| FR-017 | Grade submissions | UC-007 | US-011 | Grade | grades | PUT /api/grades/{id} | Teacher → Grade |
| FR-019 | Calculate course averages | UC-008 | US-013 | GradeCalculator | grades, enrollments | GET /api/courses/{id}/grades | Student → Grades |
| FR-020 | Display grades in real time | UC-008 | US-013 | Grade | grades | GET /api/courses/{id}/grades | Student → Grades |
| FR-021 | Generate report cards | UC-009 | US-014 | ReportCard | report_cards | POST /api/report-cards | Teacher → Reports |
| FR-023 | Parent views child data | UC-010 | US-016 | Parent, Student | parent_student_link | GET /api/parents/{id}/children | Parent Dashboard |
| FR-024 | Link parent to student | UC-010 | US-017 | ParentStudentLink | parent_student_link | POST /api/parent-links | Admin → Users |

## 12.3 Non-Functional Requirements Traceability

| NFR ID | Requirement | Design Decision | Verification Method |
|--------|-------------|----------------|---------------------|
| NFR-001 | Page load < 2s | Redis caching for dashboards; CDN for static assets | Performance test with JMeter |
| NFR-002 | 500 concurrent users | Load balancer + horizontal scaling | Load test at 500 virtual users |
| NFR-004 | TLS 1.3 encryption | HTTPS enforced at load balancer; HSTS headers | SSL Labs scan |
| NFR-005 | bcrypt password hashing | Auth Service uses bcrypt (12 rounds) | Code review + unit test |
| NFR-006 | 30-min session timeout | JWT expiration set to 30 minutes | Automated test |
| NFR-007 | FERPA compliance | Soft deletes; audit logging; role-based access | Compliance audit checklist |
| NFR-009 | 1-hour training time | Consistent navigation; contextual help on every screen | User testing with 5 new teachers |
| NFR-010 | 3-click max depth | Navigation structure verified (Section 11.2) | Navigation audit |
| NFR-012 | 99.9% uptime | MySQL replica; app instance redundancy; health checks | Monthly uptime monitoring |
| NFR-013 | 6-hour backups | Automated MySQL backup via cron + S3 | Backup restoration drill |

---

[← Previous: UI/UX Design](./11-ui-ux-design.md) | [Back to Index](./00-index.md) | [Next: Appendices →](./13-appendices.md)
