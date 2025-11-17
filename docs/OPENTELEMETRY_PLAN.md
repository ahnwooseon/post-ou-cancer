# OpenTelemetry Instrumentation Plan

This document outlines the plan for implementing OpenTelemetry instrumentation for logging, tracing, and metrics in the Post OuCancer platform.

## Overview

OpenTelemetry provides a unified observability framework for collecting logs, traces, and metrics. This plan covers the instrumentation strategy for all services in the platform.

## Current Status

**Status**: ✅ Partially implemented

- OpenTelemetry is already configured in `PostOuCancer.ServiceDefaults`
- Basic instrumentation for ASP.NET Core and HTTP clients
- OTLP exporter configured for Aspire Dashboard

## Instrumentation Goals

1. **Distributed Tracing**: Track requests across services
2. **Structured Logging**: Consistent, queryable logs
3. **Metrics**: Performance and business metrics
4. **Correlation**: Link logs, traces, and metrics via correlation IDs

## Architecture

### Components

- **OpenTelemetry SDK**: .NET OpenTelemetry packages
- **OTLP Exporter**: Export to OpenTelemetry Collector or directly to backend
- **Aspire Dashboard**: Development observability (already configured)
- **Production Backend**: To be determined (e.g., Grafana, Datadog, Azure Monitor)

### Data Flow

```
Application → OpenTelemetry SDK → OTLP Exporter → Collector/Backend → Visualization
```

## Logging Strategy

### Structured Logging

**Status**: ⚠️ To be enhanced

**Current Implementation**:

- OpenTelemetry logging configured in `ServiceDefaults`
- Structured logging with `ILogger<T>`

**Enhancements Needed**:

- [ ] Consistent log format across all services
- [ ] Correlation IDs in all logs
- [ ] Log levels: Trace, Debug, Information, Warning, Error, Critical
- [ ] Contextual information (UserId, RequestId, etc.)

**Log Format**:

```json
{
  "timestamp": "2025-11-16T12:00:00Z",
  "level": "Information",
  "message": "User logged in",
  "traceId": "00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01",
  "spanId": "00f067aa0ba902b7",
  "userId": "uuid",
  "service": "PostOuCancer.ApiService",
  "category": "AuthenticationService"
}
```

### Log Categories

- **Security**: Authentication, authorization, security events
- **Performance**: Slow queries, timeouts, performance issues
- **Business**: User actions, content creation, votes
- **Errors**: Exceptions, failures, retries
- **Infrastructure**: Health checks, service startup/shutdown

### Log Retention

- **Development**: 7 days
- **Staging**: 30 days
- **Production**: 90 days (security logs: 1 year)

## Tracing Strategy

### Distributed Tracing

**Status**: ✅ Partially implemented

**Current Implementation**:

- ASP.NET Core instrumentation (HTTP requests)
- HTTP client instrumentation
- Source-based tracing for application code

**Enhancements Needed**:

- [ ] Custom spans for business operations
- [ ] Database query tracing (EF Core)
- [ ] Message queue tracing (RabbitMQ)
- [ ] External API call tracing
- [ ] Cross-service correlation

### Span Naming Convention

**Format**: `{Service}.{Operation}`

**Examples**:

- `PostOuCancer.ApiService.CreatePost`
- `PostOuCancer.ApiService.GetUserProfile`
- `PostOuCancer.Infrastructure.Database.QueryPosts`

### Trace Context Propagation

- **HTTP**: W3C Trace Context headers
- **Message Queue**: Trace context in message headers
- **Database**: Trace context in query metadata

### Custom Spans

```csharp
using System.Diagnostics;

using var activity = ActivitySource.StartActivity("CreatePost");
activity?.SetTag("post.id", postId);
activity?.SetTag("user.id", userId);
// ... operation
activity?.SetStatus(ActivityStatusCode.Ok);
```

## Metrics Strategy

### Application Metrics

**Status**: ⚠️ To be implemented

**Metrics Categories**:

1. **HTTP Metrics** (Automatic via ASP.NET Core instrumentation):
   - Request count
   - Request duration
   - Response status codes

2. **Business Metrics** (Custom):
   - [ ] Posts created per day
   - [ ] Comments per post
   - [ ] Votes per post
   - [ ] Active users
   - [ ] User registrations

3. **Performance Metrics** (Custom):
   - [ ] Database query duration
   - [ ] Cache hit rate
   - [ ] Message queue processing time
   - [ ] External API call duration

4. **Infrastructure Metrics** (Custom):
   - [ ] Health check status
   - [ ] Service uptime
   - [ ] Resource usage (CPU, memory)

### Metric Naming Convention

**Format**: `{service}.{metric_name}`

**Examples**:

- `postoucancer.api.posts.created`
- `postoucancer.api.database.query.duration`
- `postoucancer.api.cache.hit_rate`

### Custom Metrics

```csharp
using System.Diagnostics.Metrics;

private static readonly Meter Meter = new("PostOuCancer.ApiService");
private static readonly Counter<long> PostsCreated = Meter.CreateCounter<long>("posts.created");

// Usage
PostsCreated.Add(1, new KeyValuePair<string, object?>("user.id", userId));
```

## Service-Specific Instrumentation

### API Service

**Status**: ⚠️ To be enhanced

- [x] HTTP request/response tracing
- [x] HTTP client instrumentation
- [ ] Database query tracing
- [ ] Business operation spans
- [ ] Custom metrics (posts, comments, votes)
- [ ] Error tracking

### Web Frontend (Blazor)

**Status**: ⚠️ To be implemented

- [ ] Client-side tracing (JavaScript)
- [ ] User interaction tracking
- [ ] Page load performance
- [ ] Error tracking (client-side)

### Python AI Worker

**Status**: ⚠️ To be implemented

- [ ] FastAPI instrumentation
- [ ] AI model inference tracing
- [ ] Processing time metrics
- [ ] Error tracking

### Infrastructure Services

**PostgreSQL**:

- [ ] Query performance metrics
- [ ] Connection pool metrics
- [ ] Slow query logging

**Redis**:

- [ ] Cache hit/miss metrics
- [ ] Operation latency
- [ ] Memory usage

**RabbitMQ**:

- [ ] Message processing time
- [ ] Queue depth
- [ ] Consumer lag

## Correlation IDs

**Status**: ⚠️ To be implemented

- [ ] Generate correlation ID at request entry point
- [ ] Propagate correlation ID across services
- [ ] Include correlation ID in all logs, traces, and metrics
- [ ] Use W3C Trace Context for distributed tracing

**Implementation**:

```csharp
// Middleware to add correlation ID
app.Use(async (context, next) =>
{
    var correlationId = context.Request.Headers["X-Correlation-ID"].FirstOrDefault()
        ?? Activity.Current?.TraceId.ToString()
        ?? Guid.NewGuid().ToString();
    
    context.Items["CorrelationId"] = correlationId;
    context.Response.Headers["X-Correlation-ID"] = correlationId;
    
    await next();
});
```

## Sampling Strategy

**Status**: ⚠️ To be implemented

- **Development**: 100% sampling (all traces)
- **Staging**: 50% sampling
- **Production**: 10% sampling (head-based) + 100% for errors

**Configuration**:

```csharp
builder.Services.AddOpenTelemetry()
    .WithTracing(tracing => tracing
        .SetSampler(new TraceIdRatioBasedSampler(0.1)) // 10% sampling
    );
```

## Export Configuration

### Development

- **OTLP Exporter**: Aspire Dashboard (already configured)
- **Endpoint**: `https://localhost:21190` (Aspire OTLP endpoint)

### Production

**Status**: ⚠️ To be determined

**Options**:

1. **OpenTelemetry Collector**: Centralized collection and routing
2. **Grafana Cloud**: Observability platform
3. **Azure Monitor**: For Azure deployments
4. **Datadog**: APM and observability
5. **Self-hosted**: Grafana + Loki + Tempo + Prometheus

## Performance Considerations

- **Async Export**: Non-blocking export to avoid impacting application performance
- **Batching**: Batch telemetry data before export
- **Sampling**: Reduce volume in production
- **Resource Limits**: Set limits on telemetry data collection

## Security and Privacy

- **PII Redaction**: Remove personally identifiable information from logs/traces
- **Sensitive Data**: Don't log passwords, tokens, or sensitive user data
- **Access Control**: Restrict access to observability data
- **Encryption**: Encrypt telemetry data in transit

## Implementation Checklist

- [x] Basic OpenTelemetry configuration in ServiceDefaults
- [x] ASP.NET Core instrumentation
- [x] HTTP client instrumentation
- [ ] Custom spans for business operations
- [ ] Database query tracing
- [ ] Message queue tracing
- [ ] Custom metrics implementation
- [ ] Correlation ID propagation
- [ ] Log format standardization
- [ ] Sampling configuration
- [ ] Production backend selection and configuration
- [ ] PII redaction
- [ ] Performance optimization
- [ ] Documentation for developers

## References

- [OpenTelemetry .NET](https://opentelemetry.io/docs/instrumentation/net/)
- [OpenTelemetry Specification](https://opentelemetry.io/docs/specs/)
- [.NET Aspire Observability](https://learn.microsoft.com/en-us/dotnet/aspire/fundamentals/observability)
