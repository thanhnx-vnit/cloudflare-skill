## Best Practices

1. **Use `@cloudflare/containers` package** - Cleaner API than raw Durable Object methods

2. **Set appropriate `sleepAfter`** - Balance resource usage vs cold start latency
   - Short-lived jobs: "5m"
   - Session-based: "30m" - "2h"
   - Long-running services: "2h" or longer

3. **Choose routing pattern based on use case:**
   - Stateless services → `getRandom()` load balancing
   - Stateful sessions → `getByName()` with session/user ID
   - Short-lived jobs → Unique IDs with explicit lifecycle control

4. **Pass secrets securely:**
   - Use Worker Secrets or Secret Store, not hard-coded values
   - Read KV/secrets asynchronously when starting containers
   - Don't log sensitive environment variables

5. **Design for container restarts:**
   - Containers can stop after `sleepAfter`

## Common Errors

### "Container not found"

**Cause:** Container ID invalid, container deleted, or binding incorrect
**Solution:** Verify container ID correct, check binding configuration, ensure container exists

### "Container start failed"

**Cause:** Image invalid, resource limits exceeded, or initialization error
**Solution:** Validate container image, check resource limits, review initialization logs

### "Container timeout"

**Cause:** Container took too long to start or respond
**Solution:** Optimize container startup, increase timeout if needed, check container health

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Max containers per Worker | Varies by plan | Check dashboard |
| Container sleep timeout | Configurable | Via `sleepAfter` |
| Container memory | Plan dependent | Monitor usage |
| Container CPU | Shared | Performance varies |
