# 3.9 Acceptance Criteria and Story Management

## Acceptance Criteria Fundamentals

### What Are Acceptance Criteria?

**Definition:** Conditions that must be satisfied for a story to be considered "done"

**Purpose:**
- Define scope clearly
- Enable testing
- Prevent misunderstandings
- Determine when story is complete

### Given-When-Then Format

**Structure:**
```
Given [precondition/context]
When [action/event]
Then [expected outcome]
```

**Example: Submit Assignment**
```
Given I am a student enrolled in a course
When I upload an assignment file
Then the system accepts PDF, DOC, DOCX formats up to 10MB
And displays a confirmation message
And sends me an email confirmation
```

---

## School System Examples

### Story: Enroll in Course

**Acceptance Criteria:**
1. Given I am logged in as a student
   When I search for available courses
   Then I see all courses with open enrollment

2. Given I select a course to enroll
   When the course has prerequisites I haven't met
   Then I see an error message listing required prerequisites

3. Given I meet all prerequisites
   When the course is full
   Then I see an option to join the waitlist

4. Given I successfully enroll
   When enrollment is confirmed
   Then I receive a confirmation email
   And the course appears in my schedule
   And my account is charged the course fee

---

## Definition of Done (DoD)

**Story-Level DoD:**
- [ ] All acceptance criteria met
- [ ] Code reviewed
- [ ] Unit tests written and passing
- [ ] Integration tests passing
- [ ] Documentation updated
- [ ] Deployed to staging
- [ ] Product owner accepted

---

## Story Splitting Techniques

### When to Split

Split if story is:
- > 8 story points
- Spans multiple sprints
- Has independent parts
- Too risky/uncertain

### Split by Workflow Steps

**Original (13 points):**
"As a teacher, I want to create and grade assignments"

**Split:**
1. "Teacher can create assignment" (5 points)
2. "Teacher can grade submissions" (5 points)
3. "Teacher can provide feedback" (3 points)

### Split by Business Rules

**Original (8 points):**
"Student can enroll in course"

**Split:**
1. "Student can enroll in open course" (3 points)
2. "System validates prerequisites" (3 points)
3. "System handles waitlist" (2 points)

---

## Story Management in Jira

### Story Card Format

```
Title: Student Can View Grades
Story Points: 3
Priority: High
Sprint: Sprint 5

Description:
As a student
I want to view my current grades
So that I can track my academic progress

Acceptance Criteria:
- See all enrolled courses
- View grade breakdown
- See missing assignments
```

---

## Backlog Organization

### Epic → Stories → Tasks

```
Epic: Student Enrollment
├── Story: Search Courses (5 pts)
│   ├── Task: Build search API
│   ├── Task: Create search UI
│   └── Task: Add filters
├── Story: Enroll in Course (5 pts)
└── Story: Join Waitlist (3 pts)
```

---

## Practice Exercise

**Story:** "Parent can view student progress"

**Your Tasks:**
1. Write 5 acceptance criteria (Given-When-Then)
2. Define story points
3. List 3 subtasks
4. Create Definition of Done checklist

---

**Navigation:**
- **Previous:** [3.8 User Stories](3_8-user-story-fundamentals.md)
- **Next:** [3.10 Story Mapping](3_10-story-mapping-epics.md)
- **Up:** [Chapter 3 README](chapter-03-README.md)
