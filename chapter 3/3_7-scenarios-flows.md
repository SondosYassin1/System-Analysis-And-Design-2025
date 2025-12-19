# 3.7 Scenarios and Alternative Flows

## Introduction

Real systems aren't simple happy paths. Users make mistakes, networks fail, data is invalid, deadlines pass. Your use cases must handle all these scenarios.

This section teaches you to document complex flows, exceptions, and edge cases that make use cases production-ready.

---

## Types of Scenarios

### 1. Main Success Scenario (Happy Path)
Everything goes right, no exceptions

**Example: Login**
1. User enters username/password
2. System validates
3. System grants access

### 2. Alternative Flows (Variations)
Different valid paths to achieve goal

**Example 2a: Login with SSO**
- User clicks "Login with Google"
- System redirects to Google
- Google authenticates
- System grants access

### 3. Exception Flows (Errors)
Something goes wrong

**Example 2b: Invalid credentials**
- System displays error
- User re-enters credentials
- After 3 failures: System locks account

---

## Documenting Alternative Flows

### Naming Convention

**Format:** [Step Number][Letter]. [Description]

**Examples:**
- `3a. Invalid email format` (exception at step 3)
- `5a. Course full` (exception at step 5)
- `7a. Payment declined` (exception at step 7)
- `3b. User cancels` (alternative path at step 3)

### Flow Structure

```
[Number][Letter]. [Condition/Exception]:
1. System/Actor action
2. System/Actor response
3. [Resume/End point]
```

---

## School System Examples

### Complex Example: Enroll in Course

**Main Flow:**
1. Student searches course
2. System displays results
3. Student selects course
4. System checks prerequisites
5. System checks capacity
6. System checks schedule conflicts
7. Student confirms
8. System processes enrollment
9. System charges fees
10. System sends confirmation

**Alternative Flows:**

*4a. Prerequisites missing:*
1. System displays required prerequisites
2. System shows courses that satisfy prerequisites
3. Use case ends

*5a. Course is full:*
1. System displays "Course Full"
2. System offers waitlist option
   *5a1. Student joins waitlist:*
   1. System adds to waitlist
   2. System sends waitlist confirmation
   3. Use case ends
   *5a2. Student declines:*
   1. Return to step 2

*6a. Schedule conflict:*
1. System displays conflicting courses
2. Student can:
   *6a1. Drop conflicting course → Continue to step 7*
   *6a2. Cancel enrollment → Use case ends*

*9a. Payment fails:*
1. System displays payment error
2. Student updates payment method
3. Resume at step 9
   *9a1. After 3 failures:*
   1. System reserves enrollment for 24 hours
   2. System sends payment reminder
   3. Use case ends

---

## Activity Diagrams for Complex Flows

For very complex scenarios, supplement with activity diagrams:

```mermaid
graph TD
    A[Student Searches] --> B{Results Found?}
    B -->|No| C[Display "No Results"]
    B -->|Yes| D[Display Results]
    D --> E[Student Selects]
    E --> F{Prerequisites Met?}
    F -->|No| G[Show Prerequisites]
    F -->|Yes| H{Capacity Available?}
    H -->|No| I{Join Waitlist?}
    I -->|Yes| J[Add to Waitlist]
    I -->|No| D
    H -->|Yes| K{Schedule Conflict?}
    K -->|Yes| L[Show Conflicts]
    K -->|No| M[Confirm Enrollment]
```

---

## Best Practices

### ✅ Do:
- Cover all error cases
- Number alternatives consistently
- Indicate resumption points
- Keep atomic (one condition per flow)

### ❌ Don't:
- Mix multiple conditions in one flow
- Forget resumption/termination points
- Over-complicate (split complex use cases)
- Duplicate main flow logic

---

## Template: Alternative Flow

```
**[Step]a. [Exception Name]:**
1. System detects [condition]
2. System [response action]
3. Actor [corrective action]
4. [Resume at step X / Use case ends]

**[Step]b. [Another Alternative]:**
...
```

---

## Practice Exercise

**Scenario: Submit Assignment with Multiple Exceptions**

**Main Flow:**
1. Student selects assignment
2. Student uploads file
3. System validates file
4. System checks deadline
5. System saves submission

**Your Task:** Write alternative flows for:
- Invalid file type
- File too large (>10MB)
- Past deadline
- Network error during upload
- Duplicate submission attempt

---

## Coming Up Next

In **3.8 User Story Fundamentals**, shift to Agile perspective and learn user story format.

---

**Navigation:**
- **Previous:** [3.6 Detailed Specifications](3_6-detailed-specifications.md)
- **Next:** [3.8 User Story Fundamentals](3_8-user-story-fundamentals.md)
- **Up:** [Chapter 3 README](chapter-03-README.md)
