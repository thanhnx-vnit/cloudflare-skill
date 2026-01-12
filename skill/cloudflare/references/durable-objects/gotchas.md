# Durable Objects Gotchas

## Common Errors

### "Durable Object Overloaded (503 errors)"

**Cause:** Single DO exceeding ~1000 req/s throughput limit  
**Solution:** Shard across multiple DOs using random IDs or hash-based distribution

### "Storage Quota Exceeded (Write failures)"

**Cause:** DO storage exceeding 10GB limit or account quota  
**Solution:** Upgrade plan, cleanup unused alarms, or delete old data with `deleteAll()`

### "CPU Time Exceeded (Terminated)"

**Cause:** Processing exceeding 30s CPU time default limit  
**Solution:** Increase `limits.cpu_ms` in wrangler.jsonc (max 300s) or chunk work into smaller operations

### "WebSockets Disconnect on Eviction"

**Cause:** DO evicted from memory causing WebSocket connections to drop  
**Solution:** Use WebSocket hibernation API or implement reconnection logic in client

### "Migration Failed (Deploy error)"

**Cause:** Non-unique tags, non-sequential tags, or invalid class names in migration  
**Solution:** Check tag uniqueness/sequential ordering and verify class names are correct

### "RPC Method Not Found"

**Cause:** compatibility_date < 2024-04-03 preventing RPC usage  
**Solution:** Update compatibility_date to >= 2024-04-03 or use fetch() instead of RPC

### "Only One Alarm Allowed"

**Cause:** Need multiple scheduled tasks but only one alarm supported per DO  
**Solution:** Use event queue pattern to schedule multiple tasks with single alarm

### "Expensive Constructor on Every Wake"

**Cause:** Heavy initialization in constructor runs on every DO wake  
**Solution:** Use lazy initialization in methods or cache expensive operations

### "Migration Rollback Not Supported"

**Cause:** Attempting to rollback a migration after deployment  
**Solution:** Test with `--dry-run` before deploying; migrations cannot be rolled back

### "deleted_classes Destroys Data"

**Cause:** Using `deleted_classes` in migration destroys all data for that class  
**Solution:** Coordinate carefully; transfers need explicit coordination to preserve data

## Limits

| Limit | Free | Paid | Notes |
|-------|------|------|-------|
| SQLite storage per DO | 10 GB | 10 GB | Per Durable Object |
| SQLite total storage | 5 GB | Unlimited | Account-wide |
| Key+value size | 2 MB | 2 MB | SQLite storage |
| CPU time default | 30s | 30s | Configurable |
| CPU time max | 300s | 300s | Set via `limits.cpu_ms` |
| DO classes | 100 | 500 | Number of different DO classes |
| SQL columns | 100 | 100 | Per table |
| SQL statement size | 100 KB | 100 KB | Max statement size |
| WebSocket message size | 32 MiB | 32 MiB | Per message |
| Request throughput | ~1000 req/s | ~1000 req/s | Per DO (soft limit) |
| Total DOs | Unlimited | Unlimited | Account-wide |
| WebSockets | Unlimited | Unlimited | Within 128MB memory limit |
