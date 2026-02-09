# 7. UML Behavioral Models

## 7.1 Sequence Diagram: Submit Assignment

This diagram shows the interaction between objects when a student submits an assignment (UC-006).

```mermaid
sequenceDiagram
    actor S as Student
    participant UI as Web Interface
    participant AC as AssignmentController
    participant AS as AssignmentService
    participant SS as SubmissionService
    participant DB as Database
    participant ES as EmailService

    S->>UI: Click assignment link
    UI->>AC: GET /assignments/{id}
    AC->>AS: getAssignment(id)
    AS->>DB: SELECT assignment
    DB-->>AS: assignment data
    AS-->>AC: Assignment details
    AC-->>UI: Display assignment info

    S->>UI: Upload file & click Submit
    UI->>AC: POST /submissions
    AC->>SS: createSubmission(studentId, assignmentId, file)
    
    SS->>SS: validateFileType()
    SS->>SS: validateFileSize()
    SS->>AS: isDeadlinePassed(assignmentId)
    AS-->>SS: false

    SS->>DB: Check existing submission
    DB-->>SS: No previous submission
    
    SS->>DB: Store file
    DB-->>SS: filePath
    SS->>DB: INSERT submission record
    DB-->>SS: submissionId
    
    SS-->>AC: Submission confirmation
    AC-->>UI: "Assignment submitted successfully"
    UI-->>S: Show confirmation with timestamp
```

---

## 7.2 Sequence Diagram: Record Attendance

This diagram shows the interaction when a teacher records attendance for a class (UC-004).

```mermaid
sequenceDiagram
    actor T as Teacher
    participant UI as Web Interface
    participant AC as AttendanceController
    participant AS as AttendanceService
    participant DB as Database
    participant NS as NotificationService
    participant ES as EmailService

    T->>UI: Select class, click "Take Attendance"
    UI->>AC: GET /classes/{id}/attendance?date=today
    AC->>AS: getAttendanceForm(classId, date)
    AS->>DB: SELECT enrolled students
    DB-->>AS: student roster
    AS->>DB: SELECT existing attendance for date
    DB-->>AS: No existing records
    AS-->>AC: Roster with all students set to PRESENT
    AC-->>UI: Display attendance form
    
    T->>UI: Mark 2 students Absent, 1 Late
    T->>UI: Click "Save Attendance"
    UI->>AC: POST /attendance (array of records)
    AC->>AS: saveAttendance(classId, date, records[])
    
    loop For each student
        AS->>DB: INSERT attendance record
    end
    
    AS->>NS: notifyAbsentStudentParents(absentStudentIds)
    
    loop For each absent student
        NS->>DB: Get parent email for student
        DB-->>NS: parent email
        NS->>ES: sendEmail(parentEmail, absenceAlert)
    end
    
    AS-->>AC: Attendance saved
    AC-->>UI: "Attendance saved for Math 101 — Feb 9, 2026"
    UI-->>T: Show confirmation
```

---

## 7.3 Activity Diagram: Student Enrollment Process

This diagram models the complete business process when an administrator enrolls a student in a course.

```mermaid
flowchart TD
    Start([Start]) --> A[Admin selects student]
    A --> B[Admin selects course]
    B --> C{Is course full?}
    
    C -->|Yes| D[Display 'Course full' error]
    D --> End1([End])
    
    C -->|No| E{Is student already enrolled?}
    
    E -->|Yes| F[Display 'Already enrolled' error]
    F --> End2([End])
    
    E -->|No| G{Are prerequisites met?}
    
    G -->|No| H[Display prerequisite warning]
    H --> I{Admin overrides?}
    I -->|No| End3([End])
    I -->|Yes| J[Create enrollment record]
    
    G -->|Yes| J
    
    J --> K[Set status to ACTIVE]
    K --> L[Add student to class roster]
    L --> M[Send confirmation email to student]
    M --> N[Update available seats count]
    N --> End4([End])

    style C fill:#ff9800,color:#fff
    style E fill:#ff9800,color:#fff
    style G fill:#ff9800,color:#fff
    style I fill:#ff9800,color:#fff
    style J fill:#4caf50,color:#fff
```

---

## 7.4 State Machine Diagram: Assignment Lifecycle

This diagram tracks the states an assignment submission goes through from creation to completion.

```mermaid
stateDiagram-v2
    [*] --> Draft : Student saves draft

    Draft --> Submitted : Student clicks Submit
    Draft --> Draft : Student edits draft

    Submitted --> Submitted : Student resubmits before deadline
    Submitted --> Late : Deadline passes (auto-transition)
    Submitted --> UnderReview : Teacher opens submission

    Late --> UnderReview : Teacher opens submission

    UnderReview --> Graded : Teacher saves score & feedback
    UnderReview --> ReturnedForRevision : Teacher requests resubmission

    ReturnedForRevision --> Submitted : Student resubmits
    ReturnedForRevision --> Late : Deadline passes

    Graded --> [*]
```

**State Descriptions:**

| State | Description | Allowed Transitions |
|-------|-------------|---------------------|
| Draft | Student has saved work but not submitted. | Edit, Submit |
| Submitted | Work is submitted and awaiting review. | Resubmit, auto-transition to Late |
| Late | Submitted after the deadline. Flagged for teacher awareness. | Teacher review |
| Under Review | Teacher is actively evaluating the submission. | Grade, Return |
| Returned for Revision | Teacher requests changes. Student can resubmit. | Resubmit |
| Graded | Final score and feedback recorded. Terminal state. | None |

---

## 7.5 Activity Diagram: Grade Calculation

This models the business logic for calculating a student's course average (FR-019).

```mermaid
flowchart TD
    Start([Start]) --> A[Retrieve all graded assignments for student in course]
    A --> B[Group grades by category]
    B --> C{Drop lowest grade enabled?}
    
    C -->|Yes| D[Remove lowest score from each category]
    C -->|No| E[Keep all scores]
    
    D --> F[Calculate category averages]
    E --> F
    
    F --> G[Apply category weights]
    G --> H[Sum weighted averages]
    H --> I[Round to 2 decimal places]
    I --> J[Map to letter grade using grade scale]
    J --> K[Store calculated average]
    K --> End([End])

    style F fill:#2196f3,color:#fff
    style J fill:#4caf50,color:#fff
```

**Example Calculation:**

| Category | Weight | Scores | Average |
|----------|--------|--------|---------|
| Homework | 30% | 85, 90, 78 | 84.33 |
| Quizzes | 20% | 92, 88 | 90.00 |
| Exams | 50% | 75, 82 | 78.50 |

Weighted Average = (84.33 × 0.30) + (90.00 × 0.20) + (78.50 × 0.50) = 25.30 + 18.00 + 39.25 = **82.55 (B)**

---

[← Previous: Domain Model](./06-domain-model.md) | [Back to Index](./00-index.md) | [Next: Database Design →](./08-database-design.md)
