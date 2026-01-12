# Gotchas & Limits

## Common Errors

### "Worker not found"

**Cause:** Attempting to get Worker that doesn't exist in namespace  
**Solution:** Catch error and handle gracefully with 404 response: `if (e.message.startsWith("Worker not found")) return new Response("Worker not found", { status: 404 })`

### "CPU time limit exceeded"

**Cause:** User Worker exceeded configured CPU time limit  
**Solution:** Track violations in Analytics Engine and return 429 response; consider adjusting limits per customer tier

### "Hostname Routing Issues"

**Cause:** DNS proxy settings causing routing problems  
**Solution:** Use `*/*` wildcard route which works regardless of proxy settings for orange-to-orange routing

### "Bindings Lost on Update"

**Cause:** Not using `keep_bindings` flag when updating Worker  
**Solution:** Use `keep_bindings: true` in API requests to preserve existing bindings during updates

### "Tag Filtering Not Working"

**Cause:** Special characters not URL encoded in tag filters  
**Solution:** URL encode tags (e.g., `tags=production%3Ayes`) and avoid special chars like `,` and `&`

### "Deploy Failures with ES Modules"

**Cause:** Incorrect upload format for ES modules  
**Solution:** Use multipart form upload, specify `main_module` in metadata, and set file type to `application/javascript+module`

### "Static Asset Upload Failed"

**Cause:** Invalid hash format, expired token, or incorrect encoding  
**Solution:** Hash must be first 16 bytes (32 hex chars) of SHA-256, upload within 1 hour of session creation, deploy within 1 hour of upload completion, and Base64 encode file contents

### "Outbound Worker Not Intercepting Calls"

**Cause:** Outbound Workers don't intercept Durable Object or mTLS binding fetch  
**Solution:** Plan egress control accordingly; not all fetch calls are intercepted

## Limits

| Limit | Value | Notes |
|-------|-------|-------|
| Max tags per Worker | 8 | For filtering and organization |
| Upload session JWT validity | 1 hour | Must complete upload within this time |
| Completion token validity | 1 hour | Must deploy within this time after upload |
| Workers per namespace | Unlimited | Unlike regular Workers which have limits |
| User Worker mode | Untrusted | No `request.cf` access |
| User Worker cache isolation | Full | Never share cache between customers |
| Asset hash format | First 16 bytes of SHA-256 | 32 hex characters |
| Namespace recommendation | 1 per environment | Not 1 per customer |

See [README.md](./README.md), [configuration.md](./configuration.md), [api.md](./api.md), [patterns.md](./patterns.md)
