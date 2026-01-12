### Best Practices
- Keep files under size limits
- Use supported formats
- Maintain valid Service API token
- Clean up outdated content regularly
- Monitor Vectorize index limits

## Common Errors

### "Indexing failed"

**Cause:** Unsupported file format, file too large, or invalid content
**Solution:** Verify file format supported, check file size limits, ensure content valid

### "API token invalid"

**Cause:** Token expired, revoked, or wrong permissions
**Solution:** Create new API token with correct permissions (AI Search - Read, AI Search Edit)

### "Query returned no results"

**Cause:** Index not complete, query too specific, or no matching content
**Solution:** Wait for indexing to complete, broaden query terms, check indexed content

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Max file size | Varies by format | Check docs |
| Max files per instance | Varies by plan | Monitor dashboard |
| API token quota | Per account | Create with minimal perms |
| Vectorize index | Shared limits | Monitor usage |

## Configuration via Dashboard

### 1. Create Instance
```
Dashboard → AI Search → Create
→ Choose data source (R2 bucket or Website)
→ Configure settings
→ Create
```

### 2. Monitor Indexing
```
AI Search → Select instance → Overview
View: indexing status, progress, stats
```

### 3. Test Queries
```
AI Search → Select instance → Playground
→ "Search with AI" or "Search"
→ Enter query
```

### 4. Get API Token
```
AI Search → Select instance → Use AI Search → API
→ Create API Token
→ Permissions: "AI Search - Read", "AI Search Edit"
```

### 5. Connect to Application
```
AI Search → Select instance → Connect
```
