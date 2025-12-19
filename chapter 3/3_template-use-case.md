# Use Case Template

```
USE CASE: [Name - Verb Phrase]
=========================================

METADATA
---------
ID: UC-[###]
Priority: [High/Medium/Low]
Status: [Draft/Under Review/Approved]
Version: [X.Y]
Last Updated: [Date]
Author: [Name]

SUMMARY
-------
ACTORS:
  Primary: [Actor who initiates]
  Secondary: [Supporting actors/systems]

DESCRIPTION:
[1-2 sentence summary of what this use case accomplishes]

PRECONDITIONS:
- [What must be true before use case starts]
- [List all required conditions]
- [System state requirements]

POSTCONDITIONS (Success):
- [What must be true after successful completion]
- [System state changes]
- [Notifications/side effects]

TRIGGER:
[What initiates this use case - user action, time event, system event]

MAIN SUCCESS SCENARIO
---------------------
1. [Actor action]
2. [System response]
3. [Actor action]
4. [System response]
5. [Continue alternating...]
...
N. Use case ends successfully

ALTERNATIVE FLOWS
-----------------
A1: [Descriptive name] (at step X)
  Xa. [Condition that triggers this alternative]
  Xb. [Alternative steps]
  Xc. [Resume at step Y or end]

A2: [Next alternative] (at step X)
  Xa. ...

EXCEPTION FLOWS
---------------
E1: [Error condition name] (at step X)
  Xa. [What goes wrong]
  Xb. [How system detects it]
  Xc. [Error message/notification]
  Xd. [Recovery action or graceful termination]

E2: [Next exception] (at step X)
  Xa. ...

BUSINESS RULES
--------------
BR1: [Business constraint or policy]
BR2: [Validation rule]
BR3: [Calculation formula]

SPECIAL REQUIREMENTS
--------------------
Performance: [Response time, throughput]
Security: [Authentication, authorization, encryption]
Usability: [Accessibility, user experience]
Reliability: [Uptime, error handling]

ASSUMPTIONS
-----------
- [Things assumed to be true]
- [External dependencies]

OPEN ISSUES
-----------
- [Questions to resolve]
- [Decisions needed]

NOTES
-----
[Additional context, references, related use cases]

TRACE TO
--------
Requirements: [FR-001, FR-002, NFR-003]
User Stories: [US-015, US-016]
Test Cases: [TC-025, TC-026]
```

---

## Usage Guidelines

### Filling Out the Template

**Use Case Name:**
- Start with verb
- Be specific: "Submit Assignment" not "Assignment"
- User's perspective: "Withdraw Cash" not "Process Withdrawal"

**ID Convention:**
- UC-### where ### is sequential number
- Example: UC-001, UC-002, etc.
- Can add prefixes: UC-STUD-001 (student use cases)

**Priority:**
- High: Core functionality, must have
- Medium: Important but not critical
- Low: Nice to have, future enhancement

**Preconditions:**
- Must be verifiable
- Under system's control or checkable
- Examples: "User is logged in", "Product exists", "Account active"

**Main Scenario:**
- 5-20 steps typical
- Alternate actor/system
- One action per step
- Numbered for reference

**Alternative Flows:**
- Label clearly (A1, A2, etc.)
- Reference branching point
- Show return point

**Exception Flows:**
- Label clearly (E1, E2, etc.)
- Show error detection
- Include user-friendly messages
- Specify recovery

**Business Rules:**
- Separate from flows
- Reusable across use cases
- Can reference external policy documents

---

## Example: Completed Template

See section 3.5 for complete "Submit Assignment" example using this template.

---

## Tips

1. **Start Simple:** Fill in basic sections first, add detail iteratively
2. **Be Consistent:** Use same template across all use cases
3. **Keep Updated:** Increment version when changes made
4. **Review Regularly:** Validate with stakeholders
5. **Link Everything:** Maintain traceability to requirements and tests
