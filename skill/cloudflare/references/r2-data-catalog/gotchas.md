## Best Practices

### Authentication & Security
1. **Use Admin Read & Write tokens** for read/write operations
2. **Use Admin Read only tokens** for read-only query engines
3. **Store tokens securely** in environment variables, not code
4. **Rotate tokens regularly** via dashboard
5. **Use catalog-level maintenance** with service tokens for automation

### Performance Optimization
1. **Enable compaction** for all production tables
2. **Choose appropriate target file sizes**:
   - Start with 128 MB for most workloads
   - Tune based on query patterns
3. **Configure snapshot expiration** to match retention requirements
4. **Use partitioning** in table schema for large datasets
5. **Monitor query performance** and adjust compaction settings

### Data Modeling
1. **Use namespaces** to organize tables

## Common Errors

### "Token invalid"

**Cause:** Token expired, revoked, or insufficient permissions
**Solution:** Create new token with correct permissions (Admin Read & Write), verify token not expired

### "Compaction failed"

**Cause:** Invalid target file size, insufficient resources, or table locked
**Solution:** Verify target file size appropriate, check resource availability, ensure table not locked

### "Query timeout"

**Cause:** Query too complex, large dataset, or missing indexes
**Solution:** Optimize query, add partitioning, break into smaller queries

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Max table size | Varies | Monitor compaction |
| Token quota | Per account | Admin tokens |
| Snapshot retention | Configurable | Set per table |
| Target file size | 128 MB recommended | Tune per workload |
