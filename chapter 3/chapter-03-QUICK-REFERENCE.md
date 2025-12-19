# Chapter 3 Quick Reference Guide

## Use Case Diagram Elements

### Actors (Stick Figures)
- External to system
- Represent roles, not people
- Types: Primary, Secondary, System

### Use Cases (Ovals)
- Inside system boundary
- Named with verb phrases
- Represent user goals

### System Boundary (Rectangle)
- Encloses all use cases
- Excludes all actors
- Labeled with system name

### Relationships
- **Association:** Actor ──── Use Case
- **Include:** Base ----<<include>>----> Included
- **Extend:** Extension ----<<extend>>----> Base
- **Generalization:** Child ────▷ Parent

---

## Use Case Template

```
UC-XXX: [Name]
Primary Actor: [Role]
Preconditions: [Required state]

Main Flow:
1. Actor action
2. System response
...

Alternative Flows:
Xa. Exception:
  1. Handle
  2. Resume/End

Postconditions: [Result state]
```

---

## User Story Format

```
As a [role]
I want [goal]
So that [benefit]

Acceptance Criteria:
- Criterion 1
- Criterion 2
...
```

---

## INVEST Criteria

- **I**ndependent
- **N**egotiable
- **V**aluable
- **E**stimable
- **S**mall
- **T**estable

---

## Given-When-Then

```
Given [context]
When [action]
Then [outcome]
```

---

## Key Relationships

| Relationship | Purpose | Direction | Example |
|--------------|---------|-----------|---------|
| Include | Mandatory reuse | Base → Included | Login includes Validate |
| Extend | Optional behavior | Extension → Base | Discount extends Checkout |
| Generalization | Inheritance | Child → Parent | Student → User |

---

## Story Sizing

- 1 pt: Hours
- 2 pts: 1-2 days
- 3 pts: 2-3 days
- 5 pts: 4-5 days
- 8 pts: 1-2 weeks
- 13+ pts: Split (Epic)

---

## Common Mistakes

❌ Actors inside boundary
❌ Implementation details
❌ UI element names
❌ Wrong relationship direction
❌ Inconsistent granularity
❌ Missing acceptance criteria

---

## Quick Checklist

### Diagram:
- [ ] Actors outside
- [ ] Verb-phrase names
- [ ] Clear boundary
- [ ] Proper relationships

### Specification:
- [ ] Numbered steps
- [ ] Alternative flows
- [ ] Preconditions/postconditions
- [ ] Measurable

### User Story:
- [ ] Proper format
- [ ] Acceptance criteria
- [ ] INVEST compliant
- [ ] Estimated

---

## Tools

- **Diagrams:** Lucidchart, Draw.io
- **Stories:** Jira, Trello
- **Mapping:** Miro
- **Docs:** Google Docs, Confluence

---

## Story Map Structure

```
Activities (Backbone) →
Stories (Priority) ↓
Releases (Slices) ─
```

---

**For detailed explanations, see full chapter content files.**
