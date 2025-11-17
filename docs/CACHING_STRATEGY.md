# Caching Strategy

This document outlines the caching strategy for the Post Ou Cancer platform, covering Redis usage, cache invalidation, and performance optimization.

## Overview

Caching is critical for achieving the sub-150ms P95 latency target. We use **Redis** as our distributed cache layer to store frequently accessed data, reduce database load, and improve response times.

## Principles

1. **Cache-Aside Pattern**: Application code is responsible for loading data into cache
2. **TTL-Based Expiration**: All cached items have a time-to-live (TTL) to prevent stale data
3. **Cache Invalidation**: Explicit invalidation for critical data updates
4. **Distributed Consistency**: Redis ensures cache consistency across multiple application instances
5. **Graceful Degradation**: Application continues to function if Redis is unavailable (with degraded performance)

## Cache Layers

### Layer 1: In-Memory Cache (Application Level)

**Status**: ⚠️ To be implemented

- **Use Case**: Very hot data, session data, request-scoped data
- **Implementation**: `IMemoryCache` (ASP.NET Core built-in)
- **TTL**: Short (1-5 minutes)
- **Scope**: Per application instance

**Example**:
```csharp
// Hot user profile data
var user = await _memoryCache.GetOrCreateAsync($"user:{userId}", async entry =>
{
    entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
    return await _userRepository.GetByIdAsync(userId);
});
```

### Layer 2: Distributed Cache (Redis)

**Status**: ⚠️ To be implemented

- **Use Case**: Shared data across instances, rate limiting, session storage
- **Implementation**: `StackExchange.Redis` or `Microsoft.Extensions.Caching.StackExchangeRedis`
- **TTL**: Medium to long (5 minutes to 24 hours)
- **Scope**: All application instances

**Example**:
```csharp
// Cached post data
var post = await _distributedCache.GetStringAsync($"post:{postId}");
if (post == null)
{
    post = await _postRepository.GetByIdAsync(postId);
    await _distributedCache.SetStringAsync(
        $"post:{postId}",
        JsonSerializer.Serialize(post),
        new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = TimeSpan.FromHours(1)
        });
}
```

## Cache Categories

### 1. Read-Only Data (Long TTL)

**Examples**:
- Community metadata
- Configuration settings
- Static reference data

**TTL**: 24 hours
**Invalidation**: Manual (on configuration changes)

### 2. User Data (Medium TTL)

**Examples**:
- User profiles
- User preferences
- User reputation scores

**TTL**: 1 hour
**Invalidation**: On user update events

### 3. Content Data (Short TTL)

**Examples**:
- Post content
- Comment threads
- Feed data

**TTL**: 5-15 minutes
**Invalidation**: On content update/delete events

### 4. Computed Data (Variable TTL)

**Examples**:
- Aggregated statistics
- Trending posts
- Search results

**TTL**: 1-5 minutes
**Invalidation**: Time-based (TTL)

### 5. Rate Limiting (Short TTL)

**Examples**:
- API rate limit counters
- Login attempt counters
- Request throttling

**TTL**: Per rate limit window (1 minute to 1 hour)
**Invalidation**: Automatic (TTL-based)

## Cache Key Naming Convention

**Format**: `{category}:{identifier}:{optional-suffix}`

**Examples**:
- `user:profile:{userId}`
- `post:content:{postId}`
- `community:metadata:{communityId}`
- `feed:user:{userId}:page:{pageNumber}`
- `ratelimit:api:{userId}:{endpoint}`

## Cache Invalidation Strategy

### Event-Driven Invalidation

**Status**: ⚠️ To be implemented

When data is updated, invalidate related cache entries:

```csharp
// On post update
public async Task Handle(PostUpdatedEvent evt)
{
    // Invalidate post cache
    await _cache.RemoveAsync($"post:content:{evt.PostId}");
    
    // Invalidate feed caches that might contain this post
    await _cache.RemoveByPatternAsync($"feed:*:page:*");
    
    // Invalidate user's post list cache
    await _cache.RemoveAsync($"user:posts:{evt.AuthorId}");
}
```

### Pattern-Based Invalidation

**Status**: ⚠️ To be implemented

Use Redis pattern matching to invalidate multiple keys:

```csharp
// Invalidate all feed caches for a user
var keys = _server.Keys(pattern: $"feed:user:{userId}:*");
foreach (var key in keys)
{
    await _cache.RemoveAsync(key);
}
```

### Cache Tags (Future Enhancement)

**Status**: 📋 Future consideration

Consider implementing cache tags for more efficient invalidation:
- Tag posts with `post:{postId}` and `user:{userId}`
- Invalidate by tag: remove all entries tagged with `user:{userId}`

## Cache Warming

**Status**: ⚠️ To be implemented

Pre-populate cache with frequently accessed data:

- **On Application Start**: Load hot community metadata
- **Background Jobs**: Pre-compute trending posts, popular content
- **User Login**: Pre-load user's feed and preferences

## Cache Monitoring

**Status**: ⚠️ To be implemented

Monitor cache performance:

- **Hit Rate**: Target >80% cache hit rate
- **Miss Rate**: Alert if miss rate >20%
- **Memory Usage**: Monitor Redis memory usage
- **Eviction Rate**: Track key evictions
- **Latency**: Monitor Redis response times

## Redis Configuration

### Connection Settings

```json
{
  "RedisOptions": {
    "Host": "localhost",
    "Port": 6379,
    "Password": null,
    "Database": 0,
    "ConnectTimeout": 5000,
    "SyncTimeout": 5000,
    "AbortOnConnectFail": false
  }
}
```

### Redis Clustering (Future)

**Status**: 📋 Future consideration

For high availability and scalability:
- Redis Cluster mode
- Redis Sentinel for failover
- Read replicas for read scaling

## Performance Optimization

### Serialization

**Status**: ⚠️ To be implemented

- Use efficient serialization (System.Text.Json)
- Compress large objects (GZIP)
- Store only necessary data in cache

### Batch Operations

**Status**: ⚠️ To be implemented

- Use Redis pipeline for multiple operations
- Batch cache reads when possible
- Use MGET for multiple key retrieval

### Cache-Aside with Background Refresh

**Status**: 📋 Future consideration

Refresh cache in background before expiration:
- Return stale data immediately
- Refresh in background
- Update cache asynchronously

## Error Handling

**Status**: ⚠️ To be implemented

- **Redis Unavailable**: Fall back to database (graceful degradation)
- **Cache Errors**: Log and continue (don't fail requests)
- **Connection Retry**: Implement exponential backoff
- **Circuit Breaker**: Stop attempting cache operations after repeated failures

## Testing

**Status**: ⚠️ To be implemented

- Unit tests with mock cache
- Integration tests with Testcontainers Redis
- Performance tests for cache hit/miss scenarios
- Load tests to verify cache effectiveness

## Best Practices

1. **Always Set TTL**: Never cache indefinitely
2. **Invalidate on Updates**: Keep cache consistent with database
3. **Monitor Hit Rates**: Optimize based on metrics
4. **Use Appropriate TTLs**: Balance freshness vs. performance
5. **Cache at Right Level**: Don't cache everything, cache what matters
6. **Handle Cache Failures**: Application must work without cache
7. **Use Compression**: For large objects
8. **Avoid Cache Stampede**: Use locks or background refresh

