# Workers Gotchas

## Common Errors

### "Too much CPU time used"

**Cause:** Worker exceeded CPU time limit (10ms standard, 30ms unbound)  
**Solution:** Use `ctx.waitUntil()` for background work, offload heavy compute to Durable Objects, or consider Workers AI for ML workloads

### "Module-Level State Lost"

**Cause:** Workers are stateless between requests; module-level variables reset unpredictably  
**Solution:** Use KV, D1, or Durable Objects for persistent state; don't rely on module-level variables

### "Body has already been used"

**Cause:** Attempting to read response body twice (bodies are streams)  
**Solution:** Clone response before reading: `response.clone()` or read once and create new Response with the text

### "Node.js module not found"

**Cause:** Node.js built-ins not available by default  
**Solution:** Use Workers APIs (e.g., R2 for file storage) or enable Node.js compat with `"compatibility_flags": ["nodejs_compat_v2"]`

### "Cannot fetch in global scope"

**Cause:** Attempting to use fetch during module initialization  
**Solution:** Move fetch calls inside handler functions (fetch, scheduled, etc.) where they're allowed

### "Subrequest depth limit exceeded"

**Cause:** Too many nested subrequests creating deep call chain  
**Solution:** Flatten request chain or use service bindings for direct Worker-to-Worker communication

## Limits

| Limit | Value | Notes |
|-------|-------|-------|
| Request size | 100 MB | Maximum incoming request size |
| Response size | Unlimited | Supports streaming |
| CPU time (standard) | 10ms | Standard Workers |
| CPU time (unbound) | 30ms | Unbound Workers |
| Subrequests | 1000 | Per request |
| KV reads | 1000 | Per request |
| KV write size | 25 MB | Maximum per write |
| Environment size | 5 MB | Total size of env bindings |

## See Also

- [Patterns](./patterns.md) - Best practices
- [API](./api.md) - Runtime APIs
- [Configuration](./configuration.md) - Setup
