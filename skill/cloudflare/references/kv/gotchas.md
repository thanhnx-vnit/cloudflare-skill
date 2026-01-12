# KV Gotchas & Troubleshooting

## Common Errors

### "Stale Read After Write"

**Cause:** Eventual consistency means writes may not be immediately visible in other regions  
**Solution:** Don't read immediately after write; return confirmation without reading or use the local value you just wrote. Writes visible immediately in same location, ≤60s globally

### "429 Rate Limit on Concurrent Writes"

**Cause:** Multiple concurrent writes to same key exceeding 1 write/second limit  
**Solution:** Use sequential writes, unique keys for concurrent operations, or implement retry with exponential backoff

### "Inefficient Multiple Gets"

**Cause:** Making multiple individual get() calls instead of bulk operation  
**Solution:** Use bulk get with array of keys: `env.USERS.get(["user:1", "user:2", "user:3"])` to reduce to 1 operation

### "Null Reference Error"

**Cause:** Attempting to use value without checking for null when key doesn't exist  
**Solution:** Check for null before using value or provide default with nullish coalescing: `(await env.MY_KV.get("config")) ?? "default-config"`

## Limits

| Limit | Value | Notes |
|-------|-------|-------|
| Key size | 512 bytes | Maximum key size |
| Value size | 25 MiB | Maximum value size |
| Metadata size | 1024 bytes | Maximum metadata per key |
| cacheTtl minimum | 60s | Minimum cache TTL |
| Write rate per key | 1 write/second | All plans |
| Propagation time | ≤60s | Global propagation time |
| Reads pricing | $0.50 per 10M | Per million reads |
| Writes pricing | $5.00 per 1M | Per million writes |
| Deletes pricing | $5.00 per 1M | Per million deletes |
| Storage pricing | $0.50 per GB-month | Per GB per month |
