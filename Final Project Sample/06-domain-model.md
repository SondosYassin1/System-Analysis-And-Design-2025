# 6. Domain Model & Class Diagram

## 6.1 Identifying Classes from Use Cases

Domain classes are discovered by extracting nouns from use case descriptions and requirements. The key technique is to examine actors, objects mentioned in scenarios, and data that must be stored.

| Source | Nouns Extracted | Candidate Class |
|--------|----------------|-----------------|
| UC-003: Enroll Student | student, course, enrollment, date | Student, Course, Enrollment |
| UC-006: Submit Assignment | student, assignment, submission, file, deadline | Assignment, Submission |
| UC-004: Record Attendance | teacher, class, student, date, status | Attendance |
| UC-007: Grade Submission | teacher, submission, score, feedback | Grade |
| UC-009: Generate Report Card | student, course, average, letter grade | ReportCard |
| FR-001: Manage Users | user, account, role, email | User |
| FR-023: Parent Portal | parent, child, link | ParentStudentLink |

After filtering out duplicates, attributes, and out-of-scope nouns, the final domain classes are identified below.

---

## 6.2 Domain Model

```mermaid
classDiagram
    class User {
        -int userId
        -string firstName
        -string lastName
        -string email
        -string passwordHash
        -Role role
        -boolean isActive
        -datetime createdAt
        +login()
        +logout()
        +updateProfile()
        +resetPassword()
    }

    class Student {
        -int studentId
        -string studentNumber
        -date dateOfBirth
        -date enrollmentDate
        +viewGrades()
        +submitAssignment()
        +viewSchedule()
    }

    class Teacher {
        -int teacherId
        -string employeeNumber
        -string department
        +recordAttendance()
        +createAssignment()
        +gradeSubmission()
        +generateReportCard()
    }

    class Parent {
        -int parentId
        -string phone
        +viewChildProgress()
    }

    class Course {
        -int courseId
        -string courseName
        -string description
        -int capacity
        -string semester
        -int academicYear
        +getEnrolledCount()
        +isFull()
    }

    class Enrollment {
        -int enrollmentId
        -date enrollmentDate
        -string status
    }

    class Assignment {
        -int assignmentId
        -string title
        -string description
        -datetime dueDate
        -decimal maxScore
        -string allowedFileTypes
        +isOverdue()
        +isOpen()
    }

    class Submission {
        -int submissionId
        -string filePath
        -datetime submittedAt
        -boolean isLate
        +upload()
        +replace()
    }

    class Grade {
        -int gradeId
        -decimal score
        -string feedback
        -datetime gradedAt
        -string category
    }

    class Attendance {
        -int attendanceId
        -date attendanceDate
        -AttendanceStatus status
        -datetime recordedAt
    }

    class ReportCard {
        -int reportCardId
        -decimal courseAverage
        -string letterGrade
        -string semester
        -datetime generatedAt
        +calculateAverage()
        +generatePDF()
    }

    %% Inheritance
    User <|-- Student
    User <|-- Teacher
    User <|-- Parent

    %% Associations
    Teacher "1" --> "*" Course : teaches
    Course "1" --> "*" Enrollment : has
    Student "1" --> "*" Enrollment : enrolls in
    Course "1" --> "*" Assignment : contains
    Student "1" --> "*" Submission : submits
    Assignment "1" --> "*" Submission : receives
    Submission "1" --> "0..1" Grade : earns
    Teacher "1" --> "*" Grade : gives
    Enrollment "1" --> "*" Attendance : tracks
    Student "1" --> "*" ReportCard : receives
    Course "1" --> "*" ReportCard : summarizes
    Parent "1" --> "*" Student : linked to
```

---

## 6.3 Class Relationship Summary

| Relationship | Type | Description |
|-------------|------|-------------|
| User → Student/Teacher/Parent | **Inheritance** | All users share authentication; each role adds specific behavior. |
| Student → Enrollment → Course | **Association** | Many-to-many resolved through the Enrollment class. A student enrolls in many courses; a course has many students. |
| Course → Assignment | **Composition** | Assignments belong to a course and cannot exist without it. Deleting a course deletes its assignments. |
| Assignment → Submission | **Aggregation** | A submission references an assignment but is owned by the student. |
| Submission → Grade | **Association (1:0..1)** | A submission may or may not be graded. Each grade belongs to exactly one submission. |
| Enrollment → Attendance | **Composition** | Attendance records are tied to a specific enrollment (student + course combination). |
| Parent → Student | **Association** | A parent can be linked to one or more students. |

## 6.4 Enumeration Types

```mermaid
classDiagram
    class Role {
        <<enumeration>>
        STUDENT
        TEACHER
        PARENT
        ADMIN
    }

    class AttendanceStatus {
        <<enumeration>>
        PRESENT
        ABSENT
        LATE
        EXCUSED
    }

    class SubmissionStatus {
        <<enumeration>>
        DRAFT
        SUBMITTED
        LATE
        GRADED
    }

    class EnrollmentStatus {
        <<enumeration>>
        ACTIVE
        DROPPED
        COMPLETED
    }
```

---

[← Previous: User Stories](./05-user-stories.md) | [Back to Index](./00-index.md) | [Next: UML Behavioral Models →](./07-uml-behavioral.md)
