# 3.11 Converting Between Use Cases and User Stories

## Introduction

Should you use use cases or user stories? Often, the answer is BOTH. Learn to convert between formats to leverage the strengths of each.

---

## Use Case → User Stories

### Conversion Strategy

**One Use Case → Multiple User Stories**

**Use Case: Enroll in Course**
- Main flow → Core story
- Each alternative flow → Additional story
- Complex steps → Separate stories

### Example Conversion

**Use Case: Submit Assignment**

**Becomes:**

**Story 1: Basic Submission**
```
As a student
I want to submit assignment files
So that I can complete coursework online

Acceptance Criteria:
- Upload PDF/DOC files
- See confirmation message
```

**Story 2: Validation**
```
As a student
I want the system to validate my submission
So that I know my file is acceptable

Acceptance Criteria:
- Check file format
- Check file size
- Display clear error messages
```

**Story 3: Late Submission**
```
As a student
I want to submit past deadline with penalty notice
So that I can get partial credit

Acceptance Criteria:
- Show late penalty
- Allow submission
- Mark as late
```

---

## User Stories → Use Case

### Aggregation Strategy

**Multiple Related Stories → One Use Case**

**Given Stories:**
1. Search courses
2. Filter by department
3. Filter by time
4. View course details

**Becomes Use Case: Browse Course Catalog**

**Main Flow:**
1. Student accesses catalog
2. System displays courses
3. Student applies filters (dept, time, etc.)
4. System shows filtered results
5. Student views course details
6. Use case ends

---

## Hybrid Approach (Recommended)

### When to Use Each

**For Backlog Management:**
- User stories in Jira/backlog

**For Complex Features:**
- Detailed use case specifications

**For Communication:**
- Use case diagrams (overview)
- User stories (implementation)

### Example Workflow

1. **Requirements Phase:**
   - Create use case diagrams
   - Identify all use cases

2. **Sprint Planning:**
   - Convert use cases to user stories
   - Estimate and prioritize

3. **Complex Features:**
   - Keep detailed use case descriptions
   - Reference from stories

---

## School System Example

### Use Case: Student Enrollment

**Converts to Epic:**
```
Epic: Student Enrollment

Stories:
- Search course catalog (3 pts)
- View course details (2 pts)
- Check prerequisites (5 pts)
- Enroll in open course (5 pts)
- Join course waitlist (3 pts)
- Receive confirmation (2 pts)
- Process payment (5 pts)

Total: 25 story points
```

### Linking

**Story references use case:**
```
Story: Enroll in Course
Related Use Case: UC-015
See detailed flow in use case specification
```

---

## Conversion Template

### Use Case → Stories

```
Use Case: [Name]

Core Story:
As a [actor]
I want to [main goal]
So that [benefit]

Additional Stories:
- [Alternative flow 1] → Story
- [Alternative flow 2] → Story
- [Complex validation] → Story
- [Exception handling] → Story
```

---

## Practice Exercise

**Given Use Case: Grade Assignment**

**Main Flow:**
1. Teacher selects course
2. Teacher selects assignment
3. Teacher views submissions
4. Teacher downloads file
5. Teacher enters grade
6. Teacher provides feedback
7. System notifies student

**Alternative Flows:**
- 5a: Apply grade curve
- 5b: Save as draft
- 7a: Delayed notification

**Your Task:** Convert to user stories (minimum 4)

---

## Key Takeaways

1. **Use cases** provide comprehensive view
2. **User stories** enable agile development
3. **Convert** based on project needs
4. **Maintain traceability** between formats

---

**Navigation:**
- **Previous:** [3.10 Story Mapping](3_10-story-mapping-epics.md)
- **Next:** [3.12 Hands-On Activities](3_12-hands-on-activities.md)
- **Up:** [Chapter 3 README](chapter-03-README.md)
