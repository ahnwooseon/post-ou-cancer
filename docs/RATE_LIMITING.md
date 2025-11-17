# Rate Limiting and DDoS Protection Policy

This document outlines the rate limiting and DDoS protection strategy for the Post Ou Cancer platform.

## Overview

Rate limiting protects the platform from abuse, ensures fair resource usage, and helps prevent DDoS attacks. This policy defines rate limits for different endpoints and user types.

## Principles

1. **Fair Usage**: Prevent abuse while allowing legitimate use
2. **Scalability**: Rate limits should scale with infrastructure
3. **User Experience**: Limits should be transparent and reasonable
4. **Security**: Protect against DDoS and brute-force attacks
5. **Flexibility**: Different limits for different endpoints and user types

## Rate Limiting Strategy

### Implementation Approach

**Status**: ⚠️ To be implemented

**Recommended Solution**: ASP.NET Core Rate Limiting Middleware

- Built-in rate limiting in .NET 7+
- Multiple algorithms: Fixed window, sliding window, token bucket, concurrency
- Redis-backed for distributed rate limiting
- Configurable per endpoint

### Rate Limiting Algorithms

1. **Fixed Window**: Simple, but can allow bursts at window boundaries
2. **Sliding Window**: More accurate, prevents bursts
3. **Token Bucket**: Allows bursts up to bucket size
4. **Concurrency**: Limits concurrent requests

**Recommendation**: Use sliding window for most endpoints, token bucket for burst-tolerant endpoints.

## Rate Limit Tiers

### Anonymous Users

**Status**: ⚠️ To be implemented

- **General API**: 100 requests per minute
- **Authentication endpoints**: 5 requests per minute (prevent brute force)
- **Content creation**: 10 posts per hour
- **Comments**: 30 comments per hour
- **Votes**: 100 votes per hour
- **Search**: 20 searches per minute

### Authenticated Users

**Status**: ⚠️ To be implemented

- **General API**: 1000 requests per minute
- **Content creation**: 50 posts per hour
- **Comments**: 200 comments per hour
- **Votes**: 500 votes per hour
- **Search**: 100 searches per minute
- **File uploads**: 10 uploads per hour

### Premium Users

**Status**: ⚠️ To be implemented (Future)

- **General API**: 5000 requests per minute
- **Content creation**: 200 posts per hour
- **Comments**: 1000 comments per hour
- **Votes**: Unlimited
- **Search**: 500 searches per minute
- **File uploads**: 50 uploads per hour

### API Keys / Service Accounts

**Status**: ⚠️ To be implemented (Future)

- **General API**: 10000 requests per minute
- Higher limits for trusted services
- Separate rate limit configuration

## Endpoint-Specific Limits

### Authentication Endpoints

**Status**: ⚠️ To be implemented

- **Login**: 5 attempts per 15 minutes per IP
- **Password Reset**: 3 requests per hour per email
- **Registration**: 3 registrations per hour per IP
- **Email Verification**: 10 requests per hour per email

### Content Endpoints

**Status**: ⚠️ To be implemented

- **Create Post**: Based on user tier
- **Update Post**: 10 updates per hour per post
- **Delete Post**: 5 deletions per hour
- **Report Content**: 10 reports per hour

### Search Endpoints

**Status**: ⚠️ To be implemented

- **Full-text search**: Based on user tier
- **Autocomplete**: 50 requests per minute
- **Suggestions**: 20 requests per minute

## Rate Limit Headers

**Status**: ⚠️ To be implemented

Return rate limit information in HTTP headers:

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 950
X-RateLimit-Reset: 1637078400
Retry-After: 60
```

## Rate Limit Response

**Status**: ⚠️ To be implemented

When rate limit is exceeded:

- **HTTP Status**: `429 Too Many Requests`
- **Response Body**:

```json
{
  "error": "Rate limit exceeded",
  "message": "You have exceeded the rate limit. Please try again later.",
  "retryAfter": 60,
  "limit": 1000,
  "remaining": 0,
  "reset": "2025-11-16T13:00:00Z"
}
```

## Distributed Rate Limiting

**Status**: ⚠️ To be implemented

For multi-instance deployments, use Redis for distributed rate limiting:

- **Redis**: Store rate limit counters
- **Key Format**: `ratelimit:{endpoint}:{identifier}`
- **Identifier**: User ID, IP address, or API key

**Implementation**:

```csharp
// Use Redis-backed rate limiting
builder.Services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = redisConnectionString;
});

builder.Services.AddRateLimiter(options =>
{
    options.GlobalLimiter = PartitionedRateLimiter.Create<HttpContext, string>(context =>
        RateLimitPartition.GetSlidingWindowLimiter(
            partitionKey: context.User.Identity?.Name ?? context.Connection.RemoteIpAddress?.ToString(),
            factory: partition => new SlidingWindowRateLimiterOptions
            {
                PermitLimit = 1000,
                Window = TimeSpan.FromMinutes(1),
                SegmentsPerWindow = 10
            }));
});
```

## DDoS Protection

### Layer 1: Infrastructure Level

**Status**: ⚠️ To be implemented

- **Cloud Provider DDoS Protection**:
  - Azure DDoS Protection (for Azure deployments)
  - AWS Shield (for AWS deployments)
  - Cloudflare (if using CDN)

- **Load Balancer**:
  - Connection rate limiting
  - Geographic restrictions (if needed)
  - IP allowlisting/blocklisting

### Layer 2: Application Level

**Status**: ⚠️ To be implemented

- **Rate Limiting**: As described above
- **Request Size Limits**: Limit request body size
- **Timeout Configuration**: Set appropriate timeouts
- **Connection Limits**: Limit concurrent connections

### Layer 3: Monitoring and Detection

**Status**: ⚠️ To be implemented

- **Anomaly Detection**: Monitor for unusual traffic patterns
- **Alerting**: Alert on potential DDoS attacks
- **Automatic Mitigation**: Auto-scale or block suspicious IPs
- **Traffic Analysis**: Analyze traffic patterns

## IP-Based Rate Limiting

**Status**: ⚠️ To be implemented

- **Per-IP Limits**: Additional limits based on IP address
- **IP Reputation**: Block known malicious IPs
- **Geographic Filtering**: Optional geographic restrictions
- **VPN/Proxy Detection**: Identify and limit VPN/proxy traffic

## Rate Limit Bypass

**Status**: ⚠️ To be implemented

- **Whitelist**: Trusted IPs or services can bypass rate limits
- **Emergency Override**: Admin override for legitimate high-volume use
- **Grace Period**: Temporary limit increase for special events

## Monitoring and Metrics

**Status**: ⚠️ To be implemented

Track the following metrics:

- Rate limit hits per endpoint
- Rate limit hits per user tier
- Top rate-limited IPs
- Rate limit effectiveness
- False positives (legitimate users blocked)

## Configuration

**Status**: ⚠️ To be implemented

Rate limits should be configurable:

- **Environment Variables**: Per-environment configuration
- **Configuration File**: `appsettings.json` for default values
- **Runtime Updates**: Ability to update limits without restart (via configuration service)

**Example Configuration**:

```json
{
  "RateLimiting": {
    "Anonymous": {
      "GeneralApi": { "PermitLimit": 100, "Window": "00:01:00" },
      "Authentication": { "PermitLimit": 5, "Window": "00:15:00" },
      "ContentCreation": { "PermitLimit": 10, "Window": "01:00:00" }
    },
    "Authenticated": {
      "GeneralApi": { "PermitLimit": 1000, "Window": "00:01:00" },
      "ContentCreation": { "PermitLimit": 50, "Window": "01:00:00" }
    }
  }
}
```

## Implementation Checklist

- [ ] Implement ASP.NET Core Rate Limiting middleware
- [ ] Configure rate limits per endpoint
- [ ] Implement distributed rate limiting with Redis
- [ ] Add rate limit headers to responses
- [ ] Configure DDoS protection at infrastructure level
- [ ] Set up monitoring and alerting
- [ ] Implement IP-based rate limiting
- [ ] Create rate limit configuration system
- [ ] Document rate limits for API users
- [ ] Test rate limiting under load
- [ ] Set up automatic scaling for DDoS mitigation

## Best Practices

1. ✅ **Start with conservative limits** and adjust based on usage
2. ✅ **Monitor rate limit effectiveness** regularly
3. ✅ **Provide clear error messages** when limits are exceeded
4. ✅ **Use distributed rate limiting** for multi-instance deployments
5. ✅ **Implement graceful degradation** during high load
6. ✅ **Log rate limit violations** for analysis
7. ✅ **Review and adjust limits** based on user feedback
8. ✅ **Document rate limits** in API documentation

## References

- [ASP.NET Core Rate Limiting](https://learn.microsoft.com/en-us/aspnet/core/performance/rate-limit)
- [OWASP Rate Limiting](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#rate-limiting)
- [DDoS Protection Best Practices](https://owasp.org/www-community/attacks/Denial_of_Service)
