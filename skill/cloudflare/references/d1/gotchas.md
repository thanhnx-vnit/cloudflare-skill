# D1 Gotchas & Troubleshooting

## Common Errors

### "SQL Injection Vulnerability"

**Cause:** Using string interpolation instead of prepared statements with bind()  
**Solution:** ALWAYS use prepared statements: `env.DB.prepare('SELECT * FROM users WHERE id = ?').bind(userId).all()` instead of string interpolation which allows attackers to inject malicious SQL

### "no such table"

**Cause:** Table doesn't exist because migrations haven't been run  
**Solution:** Run migrations first using `wrangler d1 migrations apply`

### "UNIQUE constraint failed"

**Cause:** Attempting to insert duplicate value in column with UNIQUE constraint  
**Solution:** Catch error and return 409 Conflict status code

### "Query Timeout (30s exceeded)"

**Cause:** Query execution exceeds 30 second timeout limit  
**Solution:** Break into smaller queries, add indexes to speed up queries, or reduce dataset size

### "N+1 Query Problem"

**Cause:** Making multiple individual queries in a loop instead of single optimized query  
**Solution:** Use JOIN to fetch related data in single query or use `batch()` method for multiple queries

### "Missing Indexes"

**Cause:** Queries performing full table scans without indexes  
**Solution:** Use `EXPLAIN QUERY PLAN` to check if index is used, then create index with `CREATE INDEX idx_users_email ON users(email)`

### "Boolean Type Issues"

**Cause:** SQLite uses INTEGER (0/1) not native boolean type  
**Solution:** Bind 1 or 0 instead of true/false when working with boolean values

### "Date/Time Type Issues"

**Cause:** SQLite doesn't have native DATE/TIME types  
**Solution:** Use TEXT (ISO 8601 format) or INTEGER (unix timestamp) for date/time values

## Limits

| Limit | Value | Notes |
|-------|-------|-------|
| Database size | 10 GB | Design for multiple DBs per tenant |
| Row size | 1 MB | Store large files in R2, not D1 |
| Query timeout | 30s | Break long queries into smaller chunks |
| Batch size | 10,000 statements | Split large batches |
| Local storage | `.wrangler/state/v3/d1/<database-id>.sqlite` | Test migrations locally first |
