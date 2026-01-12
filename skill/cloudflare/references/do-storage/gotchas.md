# DO Storage Gotchas & Troubleshooting

## Common Errors

### "Race Condition in Concurrent Calls"

**Cause:** Multiple concurrent storage operations initiated from same event (e.g., `Promise.all()`) are not protected by input gate  
**Solution:** Avoid concurrent storage operations within single event; input gate only serializes requests from different events, not operations within same event

### "Direct SQL Transaction Statements"

**Cause:** Using `BEGIN TRANSACTION` directly instead of transaction methods  
**Solution:** Use `this.ctx.storage.transactionSync()` for sync operations or `this.ctx.storage.transaction()` for async operations

### "Async in transactionSync"

**Cause:** Using async operations inside `transactionSync()` callback  
**Solution:** Use async `transaction()` method instead of `transactionSync()` when async operations needed

### "TypeScript Type Mismatch at Runtime"

**Cause:** Query doesn't return all fields specified in TypeScript type  
**Solution:** Ensure SQL query selects all columns that match the TypeScript type definition

### "Alarm Not Deleted with deleteAll()"

**Cause:** `deleteAll()` doesn't delete alarms automatically  
**Solution:** Call `deleteAlarm()` explicitly before `deleteAll()` to remove alarm

### "Slow Performance"

**Cause:** Using async KV API instead of sync API  
**Solution:** Use sync KV API (`ctx.storage.kv`) for better performance with simple key-value operations

### "High Billing from Storage Operations"

**Cause:** Excessive `rowsRead`/`rowsWritten` or unused objects not cleaned up  
**Solution:** Monitor `rowsRead`/`rowsWritten` metrics and ensure unused objects call `deleteAll()`

### "Durable Object Overloaded"

**Cause:** Single DO exceeding ~1K req/sec soft limit  
**Solution:** Shard across multiple DOs with random IDs or other distribution strategy

## Limits

| Limit | Value | Notes |
|-------|-------|-------|
| Max columns per table | 100 | SQL limitation |
| Max string/BLOB per row | 2 MB | SQL limitation |
| Max row size | 2 MB | SQL limitation |
| Max SQL statement size | 100 KB | SQL limitation |
| Max SQL parameters | 100 | SQL limitation |
| Max LIKE/GLOB pattern | 50 B | SQL limitation |
| SQLite storage per object | 10 GB | SQLite-backed storage |
| SQLite key+value size | 2 MB | SQLite-backed storage |
| KV storage per object | Unlimited | KV-style storage |
| KV key size | 2 KiB | KV-style storage |
| KV value size | 128 KiB | KV-style storage |
| Request throughput | ~1K req/sec | Soft limit per DO |
