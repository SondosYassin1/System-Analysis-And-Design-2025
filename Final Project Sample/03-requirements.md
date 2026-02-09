# 3. Requirements Specification

## 3.1 Functional Requirements

### User Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-001 | The system shall allow administrators to create, update, and deactivate user accounts for students, teachers, parents, and admins. | Must |
| FR-002 | The system shall authenticate users via email and password with secure login. | Must |
| FR-003 | The system shall enforce role-based access control (Student, Teacher, Parent, Admin). | Must |
| FR-004 | The system shall allow users to reset their password via email verification. | Must |
| FR-005 | The system shall allow users to update their profile information. | Should |

### Course & Enrollment

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-006 | The system shall allow administrators to create and manage courses with details (name, description, capacity, schedule). | Must |
| FR-007 | The system shall allow administrators to assign teachers to courses. | Must |
| FR-008 | The system shall allow administrators to enroll students in courses. | Must |
| FR-009 | The system shall prevent enrollment when a course has reached maximum capacity. | Must |
| FR-010 | The system shall display a course catalog that students and parents can browse. | Should |

### Attendance

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-011 | The system shall allow teachers to record daily attendance (Present, Absent, Late, Excused) for each student in their class. | Must |
| FR-012 | The system shall send an automatic email notification to parents when a student is marked absent. | Should |
| FR-013 | The system shall generate attendance reports by class, student, and date range. | Must |

### Assignments & Submissions

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-014 | The system shall allow teachers to create assignments with title, description, due date, and maximum score. | Must |
| FR-015 | The system shall allow students to submit assignments by uploading files before the due date. | Must |
| FR-016 | The system shall mark late submissions and optionally allow teachers to accept them. | Should |
| FR-017 | The system shall allow teachers to grade submissions with a numeric score and written feedback. | Must |
| FR-018 | The system shall notify students via email when their assignment is graded. | Should |

### Grading & Reports

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-019 | The system shall calculate course averages based on weighted grade categories configured by the teacher. | Must |
| FR-020 | The system shall display current grades to students and parents in real time. | Must |
| FR-021 | The system shall generate printable report cards at the end of each grading period. | Must |
| FR-022 | The system shall allow administrators to generate school-wide grade distribution reports. | Should |

### Parent Portal

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-023 | The system shall allow parents to view their linked child's attendance, grades, and assignments. | Must |
| FR-024 | The system shall allow administrators to link parent accounts to student accounts. | Must |

---

## 3.2 Non-Functional Requirements

### Performance

| ID | Requirement | Metric |
|----|-------------|--------|
| NFR-001 | The system shall load any page within 2 seconds under normal conditions. | 95th percentile < 2s with up to 200 concurrent users |
| NFR-002 | The system shall support 500 concurrent users without degradation. | Response time remains < 3s |
| NFR-003 | The system shall complete grade calculations within 1 second. | Tested per class of 40 students |

### Security

| ID | Requirement | Metric |
|----|-------------|--------|
| NFR-004 | The system shall encrypt all data in transit using TLS 1.3. | All HTTP traffic over HTTPS |
| NFR-005 | The system shall hash passwords using bcrypt with minimum 12 rounds. | No plaintext password storage |
| NFR-006 | The system shall terminate sessions after 30 minutes of inactivity. | Automatic logout enforced |
| NFR-007 | The system shall comply with FERPA for student data privacy. | Audit-verified |
| NFR-008 | The system shall log all data access and modifications with user ID and timestamp. | Audit trail retained 1 year |

### Usability

| ID | Requirement | Metric |
|----|-------------|--------|
| NFR-009 | New teachers shall be able to perform core tasks within 1 hour of training. | Measured by task completion test |
| NFR-010 | Any function shall be reachable within 3 clicks from the dashboard. | Navigation depth verified |
| NFR-011 | The system shall comply with WCAG 2.1 Level AA accessibility standards. | Automated + manual audit |

### Reliability

| ID | Requirement | Metric |
|----|-------------|--------|
| NFR-012 | The system shall maintain 99.9% availability during school hours (7 AM–6 PM). | Monthly uptime monitoring |
| NFR-013 | The system shall perform automated backups every 6 hours. | 30-day retention |
| NFR-014 | The system shall recover from failure within 4 hours (RTO). | Disaster recovery drill |

### Scalability

| ID | Requirement | Metric |
|----|-------------|--------|
| NFR-015 | The system shall scale to 5,000 users with infrastructure changes only. | No code changes required |
| NFR-016 | The system shall maintain performance with 10 years of historical data. | Load-tested with projected data volume |

---

## 3.3 Prioritization Summary (MoSCoW)

| Priority | Count | Examples |
|----------|-------|---------|
| **Must Have** | 16 | Login, attendance, grading, enrollment, report cards |
| **Should Have** | 8 | Email notifications, late submission handling, course catalog |
| **Could Have** | 0 | (Deferred to v1.1 planning) |
| **Won't Have** | — | AI tutoring, mobile native app, cafeteria management |

---

[← Previous: Stakeholders](./02-stakeholder-analysis.md) | [Back to Index](./00-index.md) | [Next: Use Case Model →](./04-use-case-model.md)
