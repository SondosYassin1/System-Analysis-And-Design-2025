# 3.5 Writing Use Case Descriptions

[← Previous: 3.4 Use Case Relationships](./3_4-use-case-relationships.md) | [Back to Chapter 3 README](./chapter-03-README.md) | [Next: 3.6 Advanced Techniques →](./3_6-advanced-use-case-techniques.md)

---

## 📖 Introduction

A diagram shows WHAT use cases exist. A description shows HOW they work. Detailed use case descriptions are essential for developers, testers, and stakeholders to understand exactly what the system should do.

---

## 🎯 Learning Objectives

- ✅ Write complete use case descriptions using standard templates
- ✅ Define clear preconditions and postconditions
- ✅ Document main success scenarios (basic flows)
- ✅ Handle alternative flows and exceptions
- ✅ Choose appropriate description level (brief vs. fully-dressed)

---

## 📋 Standard Use Case Template

### Template Structure

```
USE CASE: [Name]
ID: UC-###
ACTORS: Primary: [Actor], Secondary: [Actors]
DESCRIPTION: [Brief summary]
PRECONDITIONS: [What must be true before]
POSTCONDITIONS: [What must be true after success]
TRIGGER: [What initiates this]

MAIN SUCCESS SCENARIO:
1. [Actor action]
2. [System response]
3. [Actor action]
...
N. Use case ends in success

ALTERNATIVE FLOWS:
[Variations that still succeed]

EXCEPTION FLOWS:
[Error conditions and handling]

BUSINESS RULES:
[Constraints and policies]

SPECIAL REQUIREMENTS:
[Performance, security, etc.]

NOTES:
[Additional information]
```

---

## 📝 Complete Example: Submit Assignment

```
USE CASE: Submit Assignment
ID: UC-003
PRIORITY: High
ACTORS: 
  - Primary: Student
  - Secondary: Email Service, Instructor
DESCRIPTION: Student submits completed assignment before deadline
PRECONDITIONS:
  - Student is enrolled in the course
  - Student is logged into system
  - Assignment exists and is published
  - Submission deadline has not passed
POSTCONDITIONS (Success):
  - Assignment marked as submitted in system
  - Submission timestamp recorded
  - Instructor notified via email
  - Student receives confirmation email
  - File stored in system
TRIGGER: Student clicks "Submit Assignment" button

MAIN SUCCESS SCENARIO:
1. Student navigates to course assignments page
2. System displays list of active assignments
3. Student selects specific assignment
4. System displays assignment details and submission form
5. Student uploads assignment file
6. System validates file (type, size)
7. Student adds optional comments
8. Student clicks "Submit" button
9. System saves file to storage
10. System records submission with timestamp
11. System marks assignment as "Submitted"
12. System sends confirmation email to student
13. System sends notification email to instructor
14. System displays success message with submission details
15. Use case ends successfully

ALTERNATIVE FLOWS:
A1: Multiple file submission (step 5)
  5a. Student clicks "Add another file"
  5b. System allows up to 3 files
  5c. Student uploads additional files
  5d. Continue to step 6

A2: Draft submission (step 8)
  8a. Student clicks "Save as Draft"
  8b. System saves work without marking as submitted
  8c. System confirms draft saved
  8d. Use case ends (can resume later)

A3: Resubmission allowed (step 3)
  3a. System detects previous submission exists
  3b. System displays warning about resubmission
  3c. Student confirms resubmission
  3d. Continue to step 4
  3e. System archives previous submission

EXCEPTION FLOWS:
E1: Invalid file type (step 6)
  6a. System detects unsupported file format
  6b. System displays error: "Only PDF, DOC, DOCX allowed"
  6c. System prompts to select different file
  6d. Return to step 5

E2: File too large (step 6)
  6a. System detects file exceeds 10MB limit
  6b. System displays error with size limit
  6c. Student must compress or split file
  6d. Return to step 5

E3: Storage failure (step 9)
  9a. System unable to save file (server error)
  9b. System logs error
  9c. System displays: "Upload failed, please try again"
  9d. Return to step 5

E4: Network timeout (any step)
  Xa. Connection lost during upload
  Xb. System displays timeout message
  Xc. System offers to retry
  Xd. If draft exists, restore from draft
  Xe. Return to step 5

BUSINESS RULES:
BR1: File size maximum 10MB per file
BR2: Allowed formats: PDF, DOC, DOCX, TXT
BR3: Maximum 3 files per submission
BR4: Late submissions (if allowed) marked with penalty flag
BR5: Submission time based on server timestamp, not client
BR6: Once submitted, student cannot delete (only resubmit if allowed)

SPECIAL REQUIREMENTS:
SR1: Submission must complete within 60 seconds
SR2: System must handle 100 concurrent submissions
SR3: All file uploads must use HTTPS encryption
SR4: Submission confirmation email sent within 2 minutes

FREQUENCY: Daily (20-50 submissions per course per assignment)
NOTES:
- Instructor can configure resubmission settings per assignment
- System administrator can increase file size limit system-wide
- Mobile app follows same flow with adapted UI
```

---

## 📚 Description Levels

### 1. Brief Description (1-2 sentences)

**When to use:** Initial planning, high-level overview

```
UC-003: Submit Assignment
Student uploads completed assignment file to system before deadline.
System validates and stores file, then notifies student and instructor.
```

### 2. Casual Description (Paragraph format)

**When to use:** Stakeholder communication

```
UC-003: Submit Assignment
The student navigates to their course and selects an assignment to submit.
They upload their completed work (PDF or Word document), optionally add
comments, and click submit. The system checks the file is valid and not
too large, then saves it. Both the student and instructor receive email
confirmations. If something goes wrong, clear error messages guide the
student to fix the issue and try again.
```

### 3. Fully-Dressed Description (Complete template)

**When to use:** Development, testing (shown in previous example)

---

## ✍️ Writing Main Success Scenario

### Best Practices

**DO:**
- ✅ Number each step
- ✅ Alternate actor action / system response
- ✅ Use active voice ("Student clicks", "System displays")
- ✅ One action per step
- ✅ Be specific but technology-neutral
- ✅ Focus on WHAT happens, not HOW it's implemented

**DON'T:**
- ❌ Include implementation details ("Update database row 47")
- ❌ Use UI-specific terms ("Click the blue button on top-right")
- ❌ Make steps too granular ("Move mouse", "Click", "Type")
- ❌ Combine multiple actions ("System validates and saves and emails")

### Good vs. Bad Examples

❌ **Bad:**
```
1. User enters text into textbox ID 'username_field'
2. User enters text into textbox ID 'password_field'  
3. User clicks button with class 'btn-primary'
4. System runs SELECT query on users table
5. System compares password hashes using bcrypt
```

✅ **Good:**
```
1. User enters username and password
2. User clicks "Login" button
3. System validates credentials
4. System creates user session
5. System displays dashboard
```

### Appropriate Granularity

**Too Fine-Grained:**
```
1. Student moves mouse to assignment link
2. Student clicks mouse button
3. Browser sends HTTP GET request
4. Server processes request
5. Server queries database
```

**Too Coarse:**
```
1. Student submits assignment
2. System processes it
3. Done
```

**Just Right:**
```
1. Student selects assignment to submit
2. System displays submission form
3. Student uploads file
4. System validates and saves file
5. System confirms successful submission
```

---

## 🔀 Alternative and Exception Flows

### Alternative Flows (Happy Variations)

**Characteristics:**
- Still achieve the goal
- Expected variations
- Business decisions, not errors

**Examples:**
- Pay with credit card vs. PayPal
- Submit individual vs. group assignment
- Standard shipping vs. express

**Format:**
```
A1: [Descriptive name] (branching point)
  Xa. [What triggers this alternative]
  Xb. [Steps specific to this path]
  Xc. [Either return to main flow or end]
```

### Exception Flows (Error Handling)

**Characteristics:**
- Goal may not be achieved
- Error conditions
- System must recover gracefully

**Examples:**
- Invalid input
- Network failure
- Insufficient permissions
- Resource unavailable

**Format:**
```
E1: [Error condition] (where it occurs)
  Xa. [What goes wrong]
  Xb. [How system detects it]
  Xc. [Error message to user]
  Xd. [Recovery action or graceful termination]
```

### School System Examples

**Alternative: Submit Team Assignment**
```
A1: Team submission (step 5)
  5a. System detects assignment is marked for teams
  5b. System displays team member selection
  5c. Student selects team members (2-4)
  5d. System validates all members enrolled
  5e. Continue to step 6
  5f. System notifies all team members
```

**Exception: Past Deadline**
```
E1: Late submission (step 4)
  4a. System detects current time > deadline
  4b. System checks if late submissions allowed
  4c. If not allowed:
      - Display error: "Deadline passed"
      - Show instructor contact info
      - Use case terminates
  4d. If allowed with penalty:
      - Display warning about penalty
      - Student confirms acceptance
      - Continue with penalty flag set
```

---

## 📊 Preconditions and Postconditions

### Preconditions

**What must be TRUE before use case starts**

**Good Preconditions:**
- User is logged in
- Product exists in catalog
- Account has sufficient balance
- User has required permissions

**Bad Preconditions:**
- User has opened browser (too low-level)
- User wants to buy product (not verifiable)

### Postconditions

**What must be TRUE after successful completion**

**Success Postconditions:**
- Order is placed in system
- Inventory is decremented
- Payment is processed
- Customer receives confirmation

**Failure Postconditions (optional):**
- System state unchanged
- Error logged
- User notified of failure

---

## 💡 Key Takeaways

✅ **Use standard templates for consistency**

✅ **Main scenario shows ideal path**

✅ **Alternative flows = successful variations**

✅ **Exception flows = error handling**

✅ **Preconditions = required starting state**

✅ **Postconditions = guaranteed ending state**

✅ **Focus on WHAT, not HOW implementation works**

---

[← Previous: 3.4 Use Case Relationships](./3_4-use-case-relationships.md) | [Back to Chapter 3 README](./chapter-03-README.md) | [Next: 3.6 Advanced Techniques →](./3_6-advanced-use-case-techniques.md)
