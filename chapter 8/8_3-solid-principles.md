# 8.3 SOLID Principles

[← Previous: 8.2 Cohesion and Coupling](./8_2-cohesion-coupling.md) | [Back to Chapter 8](./chapter-08-README.md) | [Next: 8.4 Creational Patterns →](./8_4-design-patterns-creational.md)

---

## Learning Objectives

- Understand and apply each of the five SOLID principles
- Recognize violations of SOLID principles in existing code
- Refactor code to follow SOLID principles
- Know when (and when not) to apply each principle

**Estimated Time:** 45 minutes

---

## What is SOLID?

**SOLID** is an acronym for five design principles that help create maintainable, flexible, and understandable software:

| Letter | Principle | One-Line Summary |
|--------|-----------|------------------|
| **S** | Single Responsibility | One class = One job |
| **O** | Open/Closed | Extend, don't modify |
| **L** | Liskov Substitution | Subtypes work as parents |
| **I** | Interface Segregation | Small, focused interfaces |
| **D** | Dependency Inversion | Depend on abstractions |

---

## S - Single Responsibility Principle (SRP)

### The Principle

> **A class should have only one reason to change.**

### Violation Example

```csharp
// ❌ Violates SRP - Multiple responsibilities
public class Student
{
    public string Name { get; set; }
    public List<Grade> Grades { get; set; }
    
    // Responsibility 1: Student data
    public void UpdateContactInfo(string email) { ... }
    
    // Responsibility 2: GPA calculation
    public decimal CalculateGPA() { ... }
    
    // Responsibility 3: Report generation
    public string GenerateTranscript() { ... }
    
    // Responsibility 4: Email sending
    public void SendTranscriptByEmail(string to) { ... }
}
```

### Fixed Version

```csharp
// ✅ Each class has one responsibility
public class Student
{
    public string Name { get; set; }
    public List<Grade> Grades { get; set; }
    public void UpdateContactInfo(string email) { ... }
}

public class GPACalculator
{
    public decimal Calculate(IEnumerable<Grade> grades) { ... }
}

public class TranscriptGenerator
{
    public string Generate(Student student) { ... }
}

public class EmailService
{
    public void Send(string to, string subject, string body) { ... }
}
```

---

## O - Open/Closed Principle (OCP)

### The Principle

> **Software entities should be open for extension but closed for modification.**

### Violation Example

```csharp
// ❌ Violates OCP - Must modify to add new grade types
public class GPACalculator
{
    public decimal Calculate(Student student)
    {
        foreach (var grade in student.Grades)
        {
            switch (grade.Type)
            {
                case GradeType.Regular: // existing
                case GradeType.Honors:  // existing
                case GradeType.AP:      // existing
                // Must modify to add IB!
            }
        }
    }
}
```

### Fixed Version

```csharp
// ✅ Extend without modifying
public interface IGradeWeightStrategy
{
    decimal GetWeight(Grade grade);
}

public class RegularGradeStrategy : IGradeWeightStrategy
{
    public decimal GetWeight(Grade grade) => grade.Points;
}

public class APGradeStrategy : IGradeWeightStrategy
{
    public decimal GetWeight(Grade grade) => grade.Points + 1.0m;
}

// Adding IB: Just create new class, no modification!
public class IBGradeStrategy : IGradeWeightStrategy
{
    public decimal GetWeight(Grade grade) => grade.Points + 1.0m;
}
```

---

## L - Liskov Substitution Principle (LSP)

### The Principle

> **Subtypes must be substitutable for their base types.**

### Violation Example

```csharp
// ❌ Violates LSP - Square breaks Rectangle behavior
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }
    public int Area() => Width * Height;
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = value; base.Height = value; }
    }
    public override int Height
    {
        set { base.Height = value; base.Width = value; }
    }
}

// This breaks!
void Resize(Rectangle rect)
{
    rect.Width = 5;
    rect.Height = 10;
    // Expects 50, but Square gives 100!
}
```

### Fixed Version

```csharp
// ✅ Proper abstraction
public interface IShape
{
    int Area();
}

public class Rectangle : IShape
{
    public int Width { get; set; }
    public int Height { get; set; }
    public int Area() => Width * Height;
}

public class Square : IShape
{
    public int Side { get; set; }
    public int Area() => Side * Side;
}
```

---

## I - Interface Segregation Principle (ISP)

### The Principle

> **Clients should not be forced to depend on interfaces they don't use.**

### Violation Example

```csharp
// ❌ Violates ISP - Fat interface
public interface IStudentService
{
    Student GetStudent(int id);
    void CreateStudent(Student s);
    void UpdateStudent(Student s);
    void DeleteStudent(int id);
    decimal CalculateGPA(int id);
    Report GenerateTranscript(int id);
    void SendNotification(int id, string msg);
}

// This class only needs reading!
public class ReportViewer : IStudentService
{
    public Student GetStudent(int id) { ... }
    public Report GenerateTranscript(int id) { ... }
    
    // Forced to implement these!
    public void CreateStudent(Student s) => throw new NotImplementedException();
    public void UpdateStudent(Student s) => throw new NotImplementedException();
    // ... many more NotImplementedException
}
```

### Fixed Version

```csharp
// ✅ Segregated interfaces
public interface IStudentReader
{
    Student GetStudent(int id);
}

public interface IStudentWriter
{
    void CreateStudent(Student s);
    void UpdateStudent(Student s);
    void DeleteStudent(int id);
}

public interface IGPACalculator
{
    decimal CalculateGPA(int id);
}

// Now only implement what you need
public class ReportViewer : IStudentReader
{
    public Student GetStudent(int id) { ... }
}
```

---

## D - Dependency Inversion Principle (DIP)

### The Principle

> **High-level modules should not depend on low-level modules. Both should depend on abstractions.**

### Violation Example

```csharp
// ❌ Violates DIP - Direct dependencies
public class GradeReportService
{
    private SqlGradeRepository _repository = new SqlGradeRepository();
    private PdfGenerator _pdfGenerator = new PdfGenerator();
    private SmtpEmailSender _emailSender = new SmtpEmailSender();
    
    public void SendReport(int studentId, string email)
    {
        var grades = _repository.GetGrades(studentId);
        var pdf = _pdfGenerator.Generate(grades);
        _emailSender.Send(email, pdf);
    }
}
```

### Fixed Version

```csharp
// ✅ Depend on abstractions
public interface IGradeRepository
{
    IEnumerable<Grade> GetGrades(int studentId);
}

public interface IReportGenerator
{
    byte[] Generate(IEnumerable<Grade> grades);
}

public interface IEmailSender
{
    void Send(string to, byte[] attachment);
}

public class GradeReportService
{
    private readonly IGradeRepository _repository;
    private readonly IReportGenerator _generator;
    private readonly IEmailSender _emailSender;
    
    public GradeReportService(
        IGradeRepository repository,
        IReportGenerator generator,
        IEmailSender emailSender)
    {
        _repository = repository;
        _generator = generator;
        _emailSender = emailSender;
    }
    
    public void SendReport(int studentId, string email)
    {
        var grades = _repository.GetGrades(studentId);
        var pdf = _generator.Generate(grades);
        _emailSender.Send(email, pdf);
    }
}
```

---

## Additional Principles: DRY, KISS, YAGNI

### DRY - Don't Repeat Yourself

```csharp
// ❌ Duplicated logic
public class CourseService
{
    public bool CanEnroll(Student s, Course c)
    {
        return s.GPA >= 2.0 && s.Credits < 18 && !s.HasHolds;
    }
}

public class RegistrationService
{
    public bool CheckEligibility(Student s, Course c)
    {
        return s.GPA >= 2.0 && s.Credits < 18 && !s.HasHolds; // Same!
    }
}

// ✅ Single source of truth
public class EnrollmentRules
{
    public bool IsEligible(Student student)
    {
        return student.GPA >= MinGPA && 
               student.Credits < MaxCredits && 
               !student.HasHolds;
    }
}
```

### KISS - Keep It Simple, Stupid

```csharp
// ❌ Over-engineered
public class GradeConverter
{
    private readonly IGradeStrategy _strategy;
    private readonly IGradeFactory _factory;
    private readonly IGradeValidator _validator;
    // ... complex code
}

// ✅ Simple and clear
public class GradeConverter
{
    public string ToLetter(decimal grade) => grade switch
    {
        >= 90 => "A",
        >= 80 => "B",
        >= 70 => "C",
        >= 60 => "D",
        _ => "F"
    };
}
```

### YAGNI - You Aren't Gonna Need It

```csharp
// ❌ Building for imaginary future
public class Student
{
    public string FirstName { get; set; }
    public string MiddleName { get; set; }     // Never used
    public string MaidenName { get; set; }     // Never used
    public string PhoneticName { get; set; }   // "Might need it"
}

// ✅ Only what's needed now
public class Student
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

---

## SOLID Summary

| Principle | Goal | Violation Sign |
|-----------|------|----------------|
| **SRP** | One reason to change | Class name has "And" |
| **OCP** | Extend without modify | Switch/if for types |
| **LSP** | Subtypes substitutable | Throwing in override |
| **ISP** | Focused interfaces | NotImplementedException |
| **DIP** | Depend on abstractions | `new` in business logic |

---

## Key Takeaways

✅ **SRP**: One reason to change per class

✅ **OCP**: Use interfaces and strategies for extension

✅ **LSP**: Don't break inherited behavior

✅ **ISP**: Many small interfaces beat one large one

✅ **DIP**: Inject dependencies, don't create them

✅ **DRY, KISS, YAGNI**: Don't repeat, keep simple, build only what's needed

---

## Self-Check Questions

1. **Which principle is violated?**
   ```csharp
   public class UserManager {
       public void CreateUser() { }
       public void SendEmail() { }
       public void GenerateReport() { }
   }
   ```
   <details>
   <summary>Answer</summary>
   SRP - Class has multiple responsibilities (user management, email, reporting).
   </details>

2. **Which principle is violated?**
   ```csharp
   public class Bird { public virtual void Fly() { } }
   public class Penguin : Bird { 
       public override void Fly() => throw new Exception("Can't fly!"); 
   }
   ```
   <details>
   <summary>Answer</summary>
   LSP - Penguin can't substitute for Bird because it breaks the Fly() contract.
   </details>

---

**Previous:** [← 8.2 Cohesion and Coupling](./8_2-cohesion-coupling.md)

**Next:** [8.4 Creational Patterns →](./8_4-design-patterns-creational.md)

---

*Estimated Reading Time: 45 minutes*
