# Chapter 8: Detailed Design and Component Design - Quick Reference

**SA2025 - Software Analysis and Design**

---

## 📋 Chapter Overview

| Property | Value |
|----------|-------|
| **Duration** | 4-5 hours |
| **Sections** | 9 |
| **Prerequisites** | Chapter 7 (Architecture) |
| **Deliverable** | Component Design Specification |

---

## 🔧 SOLID Principles

```
S  Single Responsibility    One class = One job
O  Open/Closed              Extend, don't modify
L  Liskov Substitution      Subtypes work as parents
I  Interface Segregation    Small, focused interfaces
D  Dependency Inversion     Depend on abstractions
```

### Quick Check Questions

| Principle | Ask Yourself |
|-----------|--------------|
| **SRP** | Can I describe it without "and"? |
| **OCP** | Can I add features without changing code? |
| **LSP** | Can any child replace the parent? |
| **ISP** | Are all interface methods used? |
| **DIP** | Am I using interfaces, not classes? |

---

## 📊 Cohesion & Coupling

### Cohesion (Internal Quality)

| Level | Description | Example |
|-------|-------------|---------|
| 🟢 Functional | Single task | GPACalculator |
| 🟢 Sequential | Pipeline | DataProcessor |
| 🟡 Communicational | Same data | StudentDataHandler |
| 🟠 Temporal | Same time | StartupInitializer |
| 🔴 Logical | Same category | AllValidators |
| 🔴 Coincidental | Random | Utilities |

### Coupling (External Quality)

| Level | Description | Example |
|-------|-------------|---------|
| 🟢 Message | Events only | Event bus |
| 🟢 Data | Pass data only | Method params |
| 🟡 Stamp | Pass objects | Whole Student |
| 🟠 Control | Pass flags | if(saveToDb) |
| 🔴 Common | Global state | Static variables |
| 🔴 Content | Access internals | obj._privateField |

**Goal:** HIGH Cohesion + LOW Coupling

---

## 🧩 Design Patterns Quick Reference

### Creational Patterns

| Pattern | Use When | Example |
|---------|----------|---------|
| **Singleton** | One instance needed | Config, Logger |
| **Factory** | Type varies runtime | CreateReport("pdf") |
| **Builder** | Many optional params | ReportBuilder |

### Structural Patterns

| Pattern | Use When | Example |
|---------|----------|---------|
| **Adapter** | Interface mismatch | LegacyWrapper |
| **Decorator** | Add features | HeaderDecorator |
| **Facade** | Simplify subsystem | EnrollmentFacade |

### Behavioral Patterns

| Pattern | Use When | Example |
|---------|----------|---------|
| **Observer** | Event notification | GradeRecorded → notify |
| **Strategy** | Swap algorithms | GPACalculationStrategy |
| **Command** | Undo/redo | GradeCommand |

---

## 🌐 REST API Quick Reference

### HTTP Methods

| Method | Purpose | Example |
|--------|---------|---------|
| GET | Read | GET /students/123 |
| POST | Create | POST /students |
| PUT | Replace | PUT /students/123 |
| PATCH | Update | PATCH /students/123 |
| DELETE | Delete | DELETE /students/123 |

### Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 404 | Not Found |
| 500 | Server Error |

---

## ⚠️ Error Handling

### Exception Hierarchy

```
SchoolException
├── StudentNotFoundException
├── CourseNotFoundException
├── EnrollmentException
│   ├── CourseFullException
│   └── PrerequisitesNotMetException
└── GradeException
    └── InvalidGradeException
```

### Log Levels

| Level | Use For |
|-------|---------|
| Trace | Detailed debugging |
| Debug | Development info |
| Info | Normal operations |
| Warning | Unusual but handled |
| Error | Failures |
| Critical | System down |

---

## 📝 Component Specification Template

```
┌─────────────────────────────────────────┐
│ COMPONENT: [Name]                       │
│ VERSION: [X.Y]                          │
├─────────────────────────────────────────┤
│ PURPOSE:                                │
│ [What this component does]              │
├─────────────────────────────────────────┤
│ PROVIDED INTERFACES:                    │
│ • [Interface 1]                         │
│ • [Interface 2]                         │
├─────────────────────────────────────────┤
│ REQUIRED INTERFACES:                    │
│ • [Interface 1]                         │
│ • [Interface 2]                         │
├─────────────────────────────────────────┤
│ PATTERNS USED:                          │
│ • [Pattern]: [Justification]            │
└─────────────────────────────────────────┘
```

---

## 🔗 Key Connections

| From | Connection to Ch.8 |
|------|-------------------|
| Ch.4 (OOP) | Classes → Components |
| Ch.5 (UML) | Class diagrams |
| Ch.7 (Arch) | Architecture → Details |

| To | How Ch.8 Helps |
|----|----------------|
| Ch.9 (UI) | Clean components |
| Ch.10 (Docs) | Specifications |

---

## ✅ Reading Order

1. **8.1** Component Fundamentals
2. **8.2** Cohesion and Coupling
3. **8.3** SOLID Principles
4. **8.4** Creational Patterns
5. **8.5** Structural & Behavioral Patterns
6. **8.6** Interface and API Design
7. **8.7** Error Handling and Logging
8. **8.8** Activities
9. **8.9** Summary

---

*Quick Reference v1.0 | January 2026*
