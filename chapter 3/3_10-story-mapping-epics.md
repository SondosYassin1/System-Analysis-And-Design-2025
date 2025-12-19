# 3.10 Story Mapping and Epic Management

## Introduction

You have dozens of user stories. How do you organize them? What order makes sense? How do you see the big picture? **Story mapping** answers these questions.

---

## What is Story Mapping?

### Definition

**Story Map:** Visual arrangement of user stories showing user journey from left to right and priority from top to bottom.

**Created by:** Jeff Patton

**Structure:**
```
Backbone (Activities) →
User Stories ↓ (Priority)
```

---

## School Management System Story Map

### Map Structure

```
┌─────────────────────────────────────────────────────────────────┐
│  Enroll    │  Attend    │  Complete   │  Track      │  Graduate │
│            │            │  Work       │  Progress   │           │
├────────────┼────────────┼─────────────┼─────────────┼───────────┤
│ Search     │ View       │ Submit      │ View        │ Request   │
│ Courses    │ Schedule   │ Assignment  │ Grades      │ Transcript│
├────────────┼────────────┼─────────────┼─────────────┼───────────┤
│ Check      │ Join       │ Take        │ Check       │ Apply for │
│ Prereqs    │ Discussion │ Quiz        │ GPA         │ Graduation│
├────────────┼────────────┼─────────────┼─────────────┼───────────┤
│ Join       │ View       │ View        │ Generate    │           │
│ Waitlist   │ Materials  │ Feedback    │ Report      │           │
└────────────┴────────────┴─────────────┴─────────────┴───────────┘
   MVP ────────────────────────────────►
         Release 1 ──────────►
                    Future ──────────►
```

---

## Epics

### What is an Epic?

**Epic:** Large user story that needs breaking down into smaller stories

**Example:**
```
Epic: Student Enrollment System
├── Search course catalog
├── Enroll in course
├── Check prerequisites
├── Handle waitlist
├── Process payment
└── Confirm enrollment
```

### Epic Template

```
Epic Name: Course Enrollment
Goal: Enable students to register for courses online
Business Value: Reduce registrar workload, improve student satisfaction

User Stories (13):
- [ ] Search courses (3 pts)
- [ ] View course details (2 pts)
- [ ] Check prerequisites (5 pts)
...

Acceptance Criteria:
- Students can enroll without visiting office
- System handles 1000 concurrent users
- Enrollment completes in < 30 seconds
```

---

## Release Planning

### MVP (Minimum Viable Product)

**Must-Have Stories:**
- User login
- Search courses
- Enroll in course
- View schedule
- Submit assignment
- Grade assignment
- View grades

### Release 1 (Weeks 4-8)

**Should-Have:**
- Take attendance
- Discussion forums
- Email notifications
- Mobile support

### Release 2 (Weeks 9-12)

**Nice-to-Have:**
- Grade analytics
- Calendar integration
- Push notifications
- Advanced reporting

---

## Story Mapping Workshop

### Process

1. **Identify Activities** (Backbone)
   - What are main user tasks?

2. **List Stories** (Under each activity)
   - What specific stories support activity?

3. **Prioritize Vertically**
   - Most important on top

4. **Slice Releases**
   - Draw horizontal lines for releases

---

## Practice Exercise

**Scenario:** Create story map for "Teacher Grading System"

**Activities to consider:**
- View submissions
- Evaluate work
- Provide feedback
- Calculate grades
- Publish results

**Your Task:**
1. Create backbone (5-7 activities)
2. Add 3-4 stories per activity
3. Prioritize top to bottom
4. Define MVP line

---

**Navigation:**
- **Previous:** [3.9 Acceptance Criteria](3_9-acceptance-criteria.md)
- **Next:** [3.11 Format Conversion](3_11-format-conversion.md)
- **Up:** [Chapter 3 README](chapter-03-README.md)
