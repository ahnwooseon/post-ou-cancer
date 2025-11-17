# User Input Validation Strategy

This document outlines the strategy for validating and sanitizing user input in the Post Ou Cancer platform to prevent security vulnerabilities and ensure data integrity.

## Principles

1. **Validate Early**: Validate input as close to the entry point as possible
2. **Validate on Both Client and Server**: Client-side for UX, server-side for security
3. **Whitelist, Not Blacklist**: Allow only known good input
4. **Sanitize Output**: Sanitize data when displaying to prevent XSS
5. **Fail Securely**: Reject invalid input with clear error messages
6. **Defense in Depth**: Multiple layers of validation

## Validation Layers

### Layer 1: Client-Side Validation

**Status**: ⚠️ To be implemented

- **Purpose**: Improve user experience, reduce server load
- **Technology**: HTML5 validation, JavaScript, Blazor validation
- **Note**: Never rely solely on client-side validation for security

**Implementation**:

- HTML5 form validation attributes (`required`, `maxlength`, `pattern`, etc.)
- Blazor validation components (MudBlazor form validation)
- JavaScript validation for complex rules

### Layer 2: API Request Validation

**Status**: ⚠️ To be implemented

- **Purpose**: Primary security layer
- **Technology**: FluentValidation, DataAnnotations, Model validation
- **Location**: API endpoints, command/query handlers

**Implementation**:

- FluentValidation validators for commands/queries
- ASP.NET Core model validation
- Custom validation attributes

### Layer 3: Business Logic Validation

**Status**: ⚠️ To be implemented

- **Purpose**: Business rules and constraints
- **Location**: Domain layer, application services
- **Examples**: Uniqueness checks, business rule validation

### Layer 4: Database Constraints

**Status**: ⚠️ To be implemented

- **Purpose**: Final layer of defense
- **Technology**: Database constraints, triggers
- **Examples**: NOT NULL, UNIQUE, CHECK constraints, foreign keys

## Input Types and Validation Rules

### Text Input

**Status**: ⚠️ To be implemented

**Validation Rules**:

- **Length**: Min/max length limits
- **Character Set**: Allow only safe characters (whitelist)
- **Encoding**: Ensure proper UTF-8 encoding
- **Sanitization**: Remove or escape dangerous characters

**Examples**:

- **Username**: 3-30 characters, alphanumeric + underscore/hyphen
- **Post Title**: 1-200 characters, allow most Unicode characters
- **Post Content**: 1-10000 characters, allow rich text (sanitized)
- **Comment**: 1-5000 characters

**Implementation**:

```csharp
public class CreatePostCommandValidator : AbstractValidator<CreatePostCommand>
{
    public CreatePostCommandValidator()
    {
        RuleFor(x => x.Title)
            .NotEmpty()
            .MaximumLength(200)
            .Matches(@"^[\p{L}\p{N}\p{P}\p{Z}]+$"); // Unicode letters, numbers, punctuation, spaces

        RuleFor(x => x.Content)
            .NotEmpty()
            .MaximumLength(10000);
    }
}
```

### Email Address

**Status**: ⚠️ To be implemented

**Validation Rules**:

- **Format**: RFC 5322 compliant email format
- **Domain**: Validate domain exists (optional, async)
- **Disposable Emails**: Block disposable email services (optional)

**Implementation**:

```csharp
RuleFor(x => x.Email)
    .NotEmpty()
    .EmailAddress()
    .MaximumLength(254); // RFC 5321 limit
```

### URLs

**Status**: ⚠️ To be implemented

**Validation Rules**:

- **Format**: Valid URL format (HTTP/HTTPS only)
- **Protocol**: Allow only HTTP and HTTPS
- **Domain**: Validate domain (optional)
- **SSRF Protection**: Block internal/private IPs

**Implementation**:

```csharp
RuleFor(x => x.LinkUrl)
    .Must(BeValidUrl)
    .When(x => !string.IsNullOrEmpty(x.LinkUrl));

private bool BeValidUrl(string url)
{
    return Uri.TryCreate(url, UriKind.Absolute, out var uri)
        && (uri.Scheme == Uri.UriSchemeHttp || uri.Scheme == Uri.UriSchemeHttps)
        && !IsInternalIp(uri.Host);
}
```

### File Uploads

**Status**: ⚠️ To be implemented

**Validation Rules**:

- **File Type**: Whitelist allowed MIME types
- **File Size**: Maximum file size limits
- **File Name**: Sanitize file names, prevent path traversal
- **Content Scanning**: Scan for malware (optional)
- **Image Validation**: Validate image dimensions, format

**Allowed Types**:

- **Images**: JPEG, PNG, GIF, WebP (max 10MB)
- **Documents**: PDF (max 50MB)
- **Videos**: MP4, WebM (max 100MB, future)

**Implementation**:

```csharp
RuleFor(x => x.File)
    .NotNull()
    .Must(BeValidFileType)
    .Must(BeWithinSizeLimit)
    .WithMessage("Invalid file type or size");

private bool BeValidFileType(IFormFile file)
{
    var allowedTypes = new[] { "image/jpeg", "image/png", "image/gif", "image/webp" };
    return allowedTypes.Contains(file.ContentType);
}

private bool BeWithinSizeLimit(IFormFile file)
{
    return file.Length <= 10 * 1024 * 1024; // 10MB
}
```

### Numeric Input

**Status**: ⚠️ To be implemented

**Validation Rules**:

- **Range**: Min/max values
- **Type**: Integer, decimal, etc.
- **Precision**: Decimal places

**Examples**:

- **Vote Score**: -1, 0, or 1
- **Pagination**: Page number >= 1, page size 1-100

### Date/Time Input

**Status**: ⚠️ To be implemented

**Validation Rules**:

- **Format**: ISO 8601 format
- **Range**: Valid date ranges (e.g., not in future for birthdate)
- **Time Zone**: Handle time zones correctly

## Sanitization

### HTML Content

**Status**: ⚠️ To be implemented

**Strategy**: Use HTML sanitization library

**Recommended Library**: `HtmlSanitizer` (NuGet package)

**Allowed HTML Tags** (for rich text posts):

- `<p>`, `<br>`, `<strong>`, `<em>`, `<u>`, `<s>`
- `<ul>`, `<ol>`, `<li>`
- `<a>` (with href validation)
- `<blockquote>`, `<code>`, `<pre>`

**Blocked**:

- `<script>`, `<iframe>`, `<object>`, `<embed>`
- Event handlers (`onclick`, `onerror`, etc.)
- JavaScript URLs (`javascript:`)

**Implementation**:

```csharp
using Ganss.Xss;

var sanitizer = new HtmlSanitizer();
sanitizer.AllowedTags.Add("p", "br", "strong", "em", "ul", "ol", "li", "a");
sanitizer.AllowedAttributes.Add("href");
sanitizer.AllowedSchemes.Add("http", "https");

var sanitized = sanitizer.Sanitize(userInput);
```

### SQL Injection Prevention

**Status**: ✅ Already protected

- **EF Core**: Uses parameterized queries by default
- **Never use string concatenation** for SQL queries
- **Use stored procedures** if needed (with parameters)

### XSS Prevention

**Status**: ⚠️ To be implemented

- **Output Encoding**: Encode output when displaying
- **Content Security Policy (CSP)**: Restrict script execution
- **HTML Sanitization**: For user-generated HTML content
- **Blazor**: Automatic encoding in Razor components

## Validation Error Handling

**Status**: ⚠️ To be implemented

**Error Response Format**:

```json
{
  "errors": {
    "title": ["Title is required", "Title must be less than 200 characters"],
    "email": ["Invalid email format"]
  },
  "message": "Validation failed"
}
```

**HTTP Status**: `400 Bad Request`

**Implementation**:

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(new ValidationProblemDetails(ModelState));
}
```

## Common Validation Patterns

### Username Validation

**Status**: ⚠️ To be implemented

- 3-30 characters
- Alphanumeric, underscore, hyphen only
- Case-insensitive uniqueness
- No reserved usernames (admin, root, etc.)

### Password Validation

**Status**: ⚠️ To be implemented

- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one number
- At least one special character
- Not in common password list

### Content Moderation

**Status**: ⚠️ To be implemented (Future)

- Profanity filter
- Spam detection
- Link validation
- AI-based content analysis

## Implementation Checklist

- [ ] Set up FluentValidation for command/query validation
- [ ] Implement client-side validation (Blazor)
- [ ] Create validators for all user inputs
- [ ] Implement HTML sanitization
- [ ] Add file upload validation
- [ ] Implement URL validation with SSRF protection
- [ ] Set up database constraints
- [ ] Create validation error response format
- [ ] Document validation rules in API documentation
- [ ] Add validation tests
- [ ] Implement content moderation (future)

## Best Practices

1. ✅ **Validate on server-side** (never trust client-side only)
2. ✅ **Use whitelist approach** (allow only known good)
3. ✅ **Sanitize output** when displaying user content
4. ✅ **Provide clear error messages** without revealing system details
5. ✅ **Log validation failures** for security monitoring
6. ✅ **Use established libraries** (FluentValidation, HtmlSanitizer)
7. ✅ **Test edge cases** (empty strings, null, very long input, special characters)
8. ✅ **Keep validation rules consistent** across layers

## References

- [OWASP Input Validation](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
- [FluentValidation](https://docs.fluentvalidation.net/)
- [ASP.NET Core Model Validation](https://learn.microsoft.com/en-us/aspnet/core/mvc/models/validation)
- [HTML Sanitizer](https://github.com/mganss/HtmlSanitizer)
