# Sections 3.6-3.10: Combined Summary

*Note: These sections provide additional depth. The core concepts from 3.1-3.5 and 3.11 cover the essential material for Chapter 3.*

---

## 3.6 Advanced Use Case Techniques

### Complex Business Rules
- Modeling conditional logic in use cases
- Handling state-dependent behavior
- Multiple actor coordination

### Concurrent and Parallel Flows
- Simultaneous user actions
- Async operations (email, notifications)
- Race conditions and synchronization

### Temporal Constraints
- Time-based triggers
- Deadlines and timeouts  
- Scheduled operations

### Use Case Realization
- Linking use cases to design elements
- Sequence diagrams from use cases
- Object interaction modeling

---

## 3.7 User Stories

### User Story Format

**Template:**
```
As a [role]
I want [feature/capability]
So that [business value/benefit]
```

**Example:**
```
As a student
I want to submit assignments online
So that I don't need to print and deliver physically
```

### INVEST Criteria

**I**ndependent - Can be developed separately  
**N**egotiable - Details can be discussed  
**V**aluable - Provides user/business value  
**E**stimable - Team can estimate effort  
**S**mall - Fits in one sprint  
**T**estable - Clear acceptance criteria  

### Acceptance Criteria

**Format:** Given-When-Then

```
Story: Submit Assignment Online

Acceptance Criteria:
1. Given I am a logged-in student
   When I upload a PDF file under 10MB
   Then the system accepts and stores my assignment

2. Given I upload a file over 10MB
   When I click submit
   Then I see error message "File too large"

3. Given I submit successfully
   When submission is complete
   Then I receive email confirmation
```

---

## 3.8 From Use Cases to User Stories

### Conversion Process

**Step 1: Identify Epics**
Use case → Epic (large user story)

```
Use Case: Enroll in Course
  ↓
Epic: Course Enrollment
```

**Step 2: Break into Stories**
Each main scenario step or alternative flow → Story

```
Epic: Course Enrollment

Stories:
1. As a student, I want to browse course catalog
2. As a student, I want to search courses by keyword
3. As a student, I want to view course details
4. As a student, I want to check prerequisites
5. As a student, I want to enroll if seats available
6. As a student, I want to join waitlist if full
```

**Step 3: Add Acceptance Criteria**
From use case preconditions, postconditions, business rules

**Step 4: Estimate Size**
Story points (1, 2, 3, 5, 8, 13)

### Story Mapping

**Visual Planning Technique:**

```
User Activity:  Browse  →  Select  →  Enroll  →  Confirm
                  ↓          ↓         ↓         ↓
Release 1:     Search    View      Enroll    Email
               Filter    Details   (basic)   Confirm

Release 2:     Advanced  Compare   Waitlist  SMS
               Search    Courses   Join      Notify

Release 3:     Recommend Ratings   Auto-     Calendar
               Courses   Reviews   Enroll    Sync
```

---

## 3.9 Use Case Validation

### Review Techniques

**1. Peer Review**
- Swap use cases with classmate
- Check against criteria
- Provide constructive feedback

**2. Stakeholder Walkthrough**
- Present use case step-by-step
- Stakeholder identifies gaps
- Record feedback

**3. Role-Playing**
- Act out the use case
- Discover missing steps
- Identify UI/UX issues

**4. Prototyping**
- Create mockups from use cases
- Validate flows visually
- Early usability testing

### Validation Checklist

**Completeness:**
- [ ] All actors identified?
- [ ] All preconditions listed?
- [ ] All postconditions listed?
- [ ] Main scenario has 5-20 steps?
- [ ] Alternative flows covered?
- [ ] Exception handling included?

**Correctness:**
- [ ] Steps in logical order?
- [ ] Each step clear and specific?
- [ ] Business rules documented?
- [ ] Consistent with requirements?

**Quality:**
- [ ] User language (not technical)?
- [ ] One action per step?
- [ ] Active voice used?
- [ ] Free of implementation details?

**Traceability:**
- [ ] Links to requirements?
- [ ] Links to stakeholders?
- [ ] ID numbers assigned?

---

## 3.10 Hands-On Activities

### Activity 1: Actor Identification

**Scenario:** University Course Registration System

**Task:** Identify all actors (15 min)

**System Description:**
Students browse and enroll in courses. Advisors approve course selections. Instructors manage their course rosters. Registrar manages course catalog and scheduling. System integrates with student information system and sends emails.

**Your Answer:**
- Primary Actors: _______________
- Secondary Actors: _______________

---

### Activity 2: Use Case Diagram Creation

**Scenario:** Online Food Delivery App

**Requirements:**
- Customers browse restaurants, place orders, track delivery
- Restaurants receive orders, update availability, mark orders ready
- Delivery drivers accept deliveries, update location, complete deliveries
- System processes payments, sends notifications
- Admin manages restaurants, drivers, monitors system

**Task:** Create complete use case diagram (30 min)

---

### Activity 3: Use Case Description

**Scenario:** Library Book Borrowing

**Task:** Write fully-dressed use case for "Borrow Book" (45 min)

**Must Include:**
- 10-15 step main scenario
- 2 alternative flows
- 2 exception flows
- All template sections

---

### Activity 4: User Story Conversion

**Given Use Case:** Student Course Enrollment (from your diagram)

**Task:** Convert to 8-10 user stories with acceptance criteria (30 min)

---

### Activity 5: Peer Review Workshop

**Process:**
1. Swap use case descriptions with partner
2. Review using validation checklist
3. Provide written feedback
4. Discuss improvements together
5. Revise your own use case

**Time:** 45 minutes

---

### Activity 6: School System Complete Model

**Final Integration:**

Create complete use case model for School Management System:
1. Actor catalog (5-7 actors)
2. Use case diagram (12-15 use cases)
3. Three fully-dressed descriptions
4. User story backlog (20 stories)

**Time:** 2-3 hours
**This becomes part of your course project!**

---

## Practice Solutions

### Activity 1 Solution:
**Primary Actors:**
- Student (enrolls in courses)
- Advisor (approves selections)
- Instructor (manages roster)
- Registrar (manages catalog)

**Secondary Actors:**
- Email Service (notifications)
- Student Information System (enrollment data)
- Payment System (if fees apply)

---

*Continue practicing with diverse domains: healthcare, banking, e-commerce, social media, transportation, etc. The more you practice, the better you'll become at quickly modeling any system!*

