# API Versioning Strategy

This document outlines the API versioning strategy for the Post Ou Cancer platform, ensuring backward compatibility and smooth evolution of the API.

## Overview

As the platform evolves, API changes must be managed carefully to maintain backward compatibility while allowing innovation. We use **URL-based versioning** with **FastEndpoints** to manage API versions.

## Principles

1. **Backward Compatibility**: Maintain at least one previous major version
2. **Clear Versioning**: Version numbers are explicit in URLs
3. **Deprecation Policy**: Provide advance notice before removing versions
4. **Documentation**: Each version is fully documented
5. **Migration Path**: Clear migration guides between versions

## Versioning Strategy

### URL-Based Versioning

**Format**: `/api/v{version}/{resource}`

**Examples**:
- `/api/v1/posts`
- `/api/v2/posts`
- `/api/v1/users/{id}/profile`

**Rationale**:
- Explicit and clear
- Easy to route and version
- Works well with FastEndpoints
- Standard REST practice

### Version Numbering

**Semantic Versioning**: `v{major}.{minor}`

- **Major Version** (`v1`, `v2`): Breaking changes
- **Minor Version** (future): Non-breaking additions (optional)

**Current Version**: `v1`

## FastEndpoints Implementation

**Status**: ⚠️ To be implemented

### Versioned Endpoint Structure

```csharp
// v1/Posts/CreatePostEndpoint.cs
namespace PostOuCancer.ApiService.Endpoints.V1.Posts;

public class CreatePostEndpoint : Endpoint<CreatePostRequest, PostResponse>
{
    public override void Configure()
    {
        Post("/api/v1/posts");
        Version(1);
        // ... other configuration
    }
}

// v2/Posts/CreatePostEndpoint.cs (future)
namespace PostOuCancer.ApiService.Endpoints.V2.Posts;

public class CreatePostEndpoint : Endpoint<CreatePostRequestV2, PostResponseV2>
{
    public override void Configure()
    {
        Post("/api/v2/posts");
        Version(2);
        // ... other configuration
    }
}
```

### Version Discovery

FastEndpoints automatically discovers and registers versioned endpoints based on the `Version()` method.

## Breaking Changes

### What Constitutes a Breaking Change?

1. **Removing endpoints**
2. **Changing request/response structure** (removing required fields, changing types)
3. **Changing HTTP methods** (POST → PUT)
4. **Changing authentication/authorization requirements**
5. **Changing error response format**

### Non-Breaking Changes

1. **Adding new endpoints**
2. **Adding optional fields** to requests/responses
3. **Adding new HTTP methods** to existing resources
4. **Performance improvements**
5. **Bug fixes** (that don't change behavior)

## Deprecation Policy

**Status**: ⚠️ To be implemented

### Deprecation Timeline

1. **Announcement**: 6 months before removal
2. **Deprecation Header**: Add `Deprecation: true` and `Sunset: {date}` headers
3. **Documentation**: Mark as deprecated in API docs
4. **Removal**: Remove after deprecation period

### Deprecation Headers

```
Deprecation: true
Sunset: Sat, 01 Jan 2026 00:00:00 GMT
Link: <https://api.postoucancer.com/docs/v2/migration>; rel="deprecation"
```

## Version Lifecycle

### Active Versions

- **v1**: Current stable version
- **v2**: Future version (when needed)

### Deprecated Versions

- None currently

### Retired Versions

- None currently

## Migration Between Versions

**Status**: ⚠️ To be implemented

### Migration Guides

Provide detailed migration guides for each version transition:

- **What Changed**: List of breaking changes
- **Before/After Examples**: Code examples
- **Migration Steps**: Step-by-step guide
- **Common Issues**: Known migration problems and solutions

### Example Migration Guide Structure

```markdown
# Migrating from v1 to v2

## Overview
This guide helps you migrate from API v1 to v2.

## Breaking Changes

### 1. Post Creation Endpoint
- **v1**: `POST /api/v1/posts`
- **v2**: `POST /api/v2/posts`
- **Changes**: 
  - `title` field renamed to `headline`
  - `content` field now supports markdown

### 2. User Profile Response
- **v1**: Returns `email` field
- **v2**: `email` field removed (privacy)

## Migration Steps

1. Update base URL to `/api/v2`
2. Update request models
3. Update response handling
4. Test thoroughly
```

## API Documentation

**Status**: ⚠️ To be implemented

### Swagger/OpenAPI

- Separate documentation per version
- Version selector in Swagger UI
- Deprecation warnings for old versions

### FastEndpoints Swagger Configuration

```csharp
builder.Services.AddSwaggerGen(options =>
{
    options.SwaggerDoc("v1", new OpenApiInfo
    {
        Title = "Post Ou Cancer API",
        Version = "v1",
        Description = "API version 1"
    });
    
    options.SwaggerDoc("v2", new OpenApiInfo
    {
        Title = "Post Ou Cancer API",
        Version = "v2",
        Description = "API version 2 (deprecated)"
    });
});
```

## Client Libraries

**Status**: 📋 Future consideration

- Provide versioned client libraries
- Auto-generate from OpenAPI specs
- Support multiple versions in same library

## Testing

**Status**: ⚠️ To be implemented

- Test all versions independently
- Test backward compatibility
- Test migration scenarios
- Integration tests for each version

## Best Practices

1. **Minimize Breaking Changes**: Design APIs carefully to avoid frequent version changes
2. **Version Early**: Don't wait until you have many breaking changes
3. **Document Everything**: Every version should be fully documented
4. **Provide Migration Tools**: Help clients migrate easily
5. **Monitor Usage**: Track which versions are used to plan deprecation
6. **Communicate Changes**: Notify API consumers of upcoming changes
7. **Support Multiple Versions**: Maintain at least 2 versions simultaneously

## Versioning Checklist

When creating a new version:

- [ ] Document all breaking changes
- [ ] Create migration guide
- [ ] Update API documentation
- [ ] Add deprecation headers to old version
- [ ] Update client libraries (if applicable)
- [ ] Notify API consumers
- [ ] Set deprecation timeline
- [ ] Test backward compatibility
- [ ] Update integration tests

