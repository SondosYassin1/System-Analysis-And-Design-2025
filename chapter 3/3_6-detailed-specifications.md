# 3.6 Writing Use Case Specifications

## Introduction

A use case diagram shows WHAT the system does. But HOW does it work? What are the exact steps? What can go wrong? This is where **detailed use case specifications** come in.

Think of the diagram as a restaurant menu (list of dishes), and the specification as the recipe (how to make each dish).

**Goal:** Learn to write complete, unambiguous use case descriptions that developers can implement and testers can validate.

---

## Use Case Template Structure

### Standard IEEE Template

**Use Case ID:** UC-001  
**Use Case Name:** Enroll in Course  
**Brief Description:** Student enrolls in available course  

**Primary Actor:** Student  
**Secondary Actors:** Email Service  
**Stakeholders:** Student, Registrar, Teacher

**Preconditions:**
- Student is logged in
- Student has not exceeded maximum course load
- Registration period is open

**Main Success Scenario:**
1. Student searches course catalog
2. System displays available courses
3. Student selects desired course
4. System checks prerequisites
5. System checks course capacity
6. Student confirms enrollment
7. System registers student
8. System sends confirmation email
9. Use case ends

**Alternative Flows:**

*4a. Prerequisites not met:*
1. System displays prerequisite requirements
2. System suggests prerequisite courses
3. Use case ends

*5a. Course is full:*
1. System displays "Course Full" message
2. System offers waitlist option
3. If student accepts: System adds to waitlist
4. Use case ends

**Postconditions:**
- Student is enrolled in course
- Enrollment record created
- Confirmation email sent
- Teacher notified of new enrollment

**Special Requirements:**
- Response time < 3 seconds
- System must handle 100 concurrent enrollments

**Technology and Data Variations:**
- Mobile and desktop interfaces supported
- Integration with student information system

**Frequency of Occurrence:** High (daily during registration)  
**Open Issues:** None

---

## School Management System Examples

### Example 1: Submit Assignment

**UC-003: Submit Assignment**

**Primary Actor:** Student  
**Preconditions:** Student enrolled in course, assignment exists

**Main Flow:**
1. Student logs into system
2. System displays enrolled courses
3. Student selects course
4. System displays assignments for course
5. Student selects assignment to submit
6. System displays submission form
7. Student uploads file(s)
8. System validates file format and size
9. System checks submission deadline
10. Student adds comments (optional)
11. Student confirms submission
12. System saves submission
13. System displays confirmation
14. System notifies teacher
15. Use case ends

**Alternative Flows:**

*8a. Invalid file format:*
1. System displays error: "Invalid file type. Accepted: PDF, DOC, DOCX"
2. Return to step 7

*8b. File too large:*
1. System displays: "File exceeds 10MB limit"
2. Return to step 7

*9a. Past deadline:*
1. System displays: "Assignment overdue. Late penalty may apply"
2. System shows penalty information
3. If student proceeds: System marks as late
4. Continue to step 10

*11a. Student cancels:*
1. System discards upload
2. Return to step 4

**Postconditions:**
- Submission recorded in database
- Teacher notified
- Timestamp recorded
- File stored securely

---

### Example 2: Grade Assignment

**UC-015: Grade Assignment**

**Primary Actor:** Teacher  
**Preconditions:** Student submitted assignment, Teacher teaches course

**Main Flow:**
1. Teacher logs in
2. System displays courses taught
3. Teacher selects course
4. System displays assignments for course
5. Teacher selects assignment to grade
6. System displays submitted assignments
7. Teacher selects submission to grade
8. System displays submission details and file
9. Teacher downloads/views submission
10. Teacher enters grade (0-100)
11. Teacher enters feedback comments
12. Teacher confirms grading
13. System saves grade and feedback
14. System notifies student
15. Use case ends

**Alternative Flows:**

*10a. Invalid grade value:*
1. System displays: "Grade must be between 0 and 100"
2. Return to step 10

*12a. Teacher wants to defer:*
1. System saves as draft
2. Student not notified yet
3. Use case ends

**Extension Points:**
- "grading complete" (step 12): Apply curve, add bonus

**Postconditions:**
- Grade recorded
- Student notified
- Grade visible in student gradebook

---

## Writing Guidelines

### Be Specific and Measurable
❌ Bad: "System displays results"  
✅ Good: "System displays search results ordered by relevance, showing title, instructor, and available seats"

### Use Active Voice
❌ Bad: "The course information is displayed by the system"  
✅ Good: "System displays course information"

### Number All Steps
✅ Good practice for traceability and testing

### Focus on Observable Behavior
❌ Bad: "System queries database and caches results"  
✅ Good: "System displays available courses"

---

## Quick Reference Template

```
**UC-XXX: [Use Case Name]**

**Actors:** [Primary], [Secondary]
**Preconditions:** [What must be true before starting]

**Main Flow:**
1. Actor does X
2. System responds with Y
3. Actor confirms Z
4. System completes action
5. Use case ends

**Alternative Flows:**
*Xa. [Exception name]:*
1. System handles exception
2. [Continue or end]

**Postconditions:** [What's true after completion]
```

---

## Practice Exercise

Write detailed specification for: **UC-005: Check Grades**

**Given:**
- Actor: Student
- Goal: View current grades for all courses

**Your task:** Write complete specification with main flow and at least 2 alternative flows.

---

## Coming Up Next

In **3.7 Scenarios and Flow Documentation**, learn advanced techniques for complex flows, decision trees, and activity diagrams.

---

**Navigation:**
- **Previous:** [3.5 Generalization](3_5-generalization-relationships.md)
- **Next:** [3.7 Scenarios and Flows](3_7-scenarios-flows.md)
- **Up:** [Chapter 3 README](chapter-03-README.md)
