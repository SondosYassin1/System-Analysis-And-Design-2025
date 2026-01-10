# 8.7 Error Handling and Logging

[← Previous: 8.6 Interface and API Design](./8_6-interface-api-design.md) | [Back to Chapter 8](./chapter-08-README.md) | [Next: 8.8 Hands-on Activities →](./8_8-hands-on-activities.md)

---

## Learning Objectives

- Design effective exception handling strategies
- Implement structured logging for production systems
- Create monitoring and alerting approaches
- Apply error handling patterns to the School Management System

**Estimated Time:** 30 minutes

---

## Exception Design Principles

### 1. Use Domain-Specific Exceptions

```csharp
// ❌ Generic exceptions
throw new Exception("Student not found");

// ✅ Domain-specific exceptions
public class StudentNotFoundException : SchoolException
{
    public int StudentId { get; }
    
    public StudentNotFoundException(int studentId)
        : base($"Student {studentId} not found", "STUDENT_NOT_FOUND")
    {
        StudentId = studentId;
    }
}
```

### 2. Exception Hierarchy

```csharp
// Base exception for the system
public class SchoolException : Exception
{
    public string ErrorCode { get; }
    
    public SchoolException(string message, string errorCode) 
        : base(message)
    {
        ErrorCode = errorCode;
    }
}

// Specific exceptions
public class StudentNotFoundException : SchoolException { }
public class CourseNotFoundException : SchoolException { }
public class EnrollmentException : SchoolException { }
public class GradeException : SchoolException { }
```

### 3. Include Context

```csharp
public class GradeValidationException : SchoolException
{
    public decimal AttemptedScore { get; }
    public int StudentId { get; }
    public int CourseId { get; }
    public IReadOnlyList<string> Errors { get; }
    
    public GradeValidationException(
        decimal score, int studentId, int courseId, IEnumerable<string> errors)
        : base($"Grade validation failed", "GRADE_VALIDATION")
    {
        AttemptedScore = score;
        StudentId = studentId;
        CourseId = courseId;
        Errors = errors.ToList().AsReadOnly();
    }
}
```

---

## The Result Pattern

Alternative to exceptions for expected failures:

```csharp
public class Result<T>
{
    public bool IsSuccess { get; }
    public T Value { get; }
    public string Error { get; }
    public string ErrorCode { get; }
    
    public static Result<T> Success(T value) => 
        new Result<T> { IsSuccess = true, Value = value };
        
    public static Result<T> Failure(string error, string code = null) => 
        new Result<T> { IsSuccess = false, Error = error, ErrorCode = code };
}

// Usage
public Result<Enrollment> Enroll(int studentId, int courseId)
{
    var student = _studentRepo.GetById(studentId);
    if (student == null)
        return Result<Enrollment>.Failure("Student not found", "STUDENT_NOT_FOUND");
    
    var course = _courseRepo.GetById(courseId);
    if (course.IsFull)
        return Result<Enrollment>.Failure("Course is full", "COURSE_FULL");
    
    var enrollment = new Enrollment(student, course);
    _enrollmentRepo.Add(enrollment);
    
    return Result<Enrollment>.Success(enrollment);
}
```

---

## Global Exception Handler

```csharp
public class GlobalExceptionMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger _logger;
    
    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            await HandleExceptionAsync(context, ex);
        }
    }
    
    private async Task HandleExceptionAsync(HttpContext context, Exception ex)
    {
        var (statusCode, response) = ex switch
        {
            StudentNotFoundException e => (404, new { 
                code = e.ErrorCode, 
                message = e.Message 
            }),
            
            EnrollmentException e => (400, new { 
                code = e.ErrorCode, 
                message = e.Message 
            }),
            
            _ => (500, new { 
                code = "INTERNAL_ERROR", 
                message = "An unexpected error occurred" 
            })
        };
        
        _logger.LogError(ex, "Request failed: {Code}", response.code);
        
        context.Response.StatusCode = statusCode;
        await context.Response.WriteAsJsonAsync(response);
    }
}
```

---

## Structured Logging

### Log Levels

| Level | When to Use | Example |
|-------|-------------|---------|
| **Trace** | Very detailed | Method entry/exit |
| **Debug** | Development | Variable values |
| **Information** | Normal ops | User logged in |
| **Warning** | Unusual | Retry succeeded |
| **Error** | Failures | Database timeout |
| **Critical** | System down | Cannot start |

### Structured vs Unstructured

```csharp
// ❌ Unstructured - hard to search
_logger.LogInformation($"Student {studentId} enrolled in {courseId}");

// ✅ Structured - searchable
_logger.LogInformation(
    "Student enrolled {@Details}",
    new { StudentId = studentId, CourseId = courseId });
```

### Example Service with Logging

```csharp
public class GradeService
{
    private readonly ILogger<GradeService> _logger;
    
    public async Task<Grade> RecordGradeAsync(RecordGradeRequest request)
    {
        _logger.LogDebug("Recording grade for {StudentId}", request.StudentId);
        
        try
        {
            if (request.Score < 0 || request.Score > 100)
            {
                _logger.LogWarning("Invalid score {Score} for {StudentId}", 
                    request.Score, request.StudentId);
                throw new InvalidGradeException(request.Score);
            }
            
            var grade = new Grade { /* ... */ };
            await _repository.AddAsync(grade);
            
            _logger.LogInformation("Grade recorded {@Grade}", new { 
                GradeId = grade.Id, 
                StudentId = grade.StudentId, 
                Score = grade.Score 
            });
            
            return grade;
        }
        catch (Exception ex) when (ex is not InvalidGradeException)
        {
            _logger.LogError(ex, "Failed to record grade for {StudentId}", 
                request.StudentId);
            throw;
        }
    }
}
```

---

## Correlation IDs

Track requests across services:

```csharp
public class CorrelationIdMiddleware
{
    public async Task InvokeAsync(HttpContext context)
    {
        var correlationId = context.Request.Headers["X-Correlation-ID"]
            .FirstOrDefault() ?? Guid.NewGuid().ToString();
        
        context.Items["CorrelationId"] = correlationId;
        
        using (_logger.BeginScope(new { CorrelationId = correlationId }))
        {
            await _next(context);
        }
    }
}
```

---

## Health Checks

```csharp
public class SchoolHealthCheck : IHealthCheck
{
    private readonly IDbConnection _db;
    
    public async Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken ct)
    {
        try
        {
            await _db.ExecuteAsync("SELECT 1");
            return HealthCheckResult.Healthy("Database OK");
        }
        catch (Exception ex)
        {
            return HealthCheckResult.Unhealthy("Database failed", ex);
        }
    }
}

// Response: GET /health
{
    "status": "Healthy",
    "checks": {
        "database": "healthy",
        "cache": "healthy"
    }
}
```

---

## Alerting Rules

| Condition | Severity | Action |
|-----------|----------|--------|
| Error rate > 5% | Critical | Page on-call |
| Response time > 2s | Warning | Investigate |
| Disk space < 20% | Warning | Notify ops |
| Database down | Critical | Page on-call |

---

## Key Takeaways

✅ **Exception Design**
- Use domain-specific exceptions
- Include relevant context
- Create exception hierarchies

✅ **Result Pattern**
- Use for expected failures
- Makes failures explicit
- Easier to handle than exceptions

✅ **Structured Logging**
- Use proper log levels
- Include structured data
- Use correlation IDs

✅ **Monitoring**
- Implement health checks
- Track key metrics
- Set up alerts

---

## Self-Check Questions

1. **When use exceptions vs Result types?**
   <details>
   <summary>Answer</summary>
   Use exceptions for truly unexpected errors (database down, null reference). Use Result for expected business failures (student not found, course full) that callers should handle.
   </details>

2. **Why include context in exceptions?**
   <details>
   <summary>Answer</summary>
   Context (IDs, values, timestamps) helps debugging. Instead of just "validation failed," you know which student, which grade, and what was wrong.
   </details>

---

**Previous:** [← 8.6 Interface and API Design](./8_6-interface-api-design.md)

**Next:** [8.8 Hands-on Activities →](./8_8-hands-on-activities.md)

---

*Estimated Reading Time: 30 minutes*
