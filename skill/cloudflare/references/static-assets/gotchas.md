## Best Practices

### 1. Use Selective Worker-First Routing

Instead of `run_worker_first = true`, use array patterns:

```jsonc
{
  "assets": {
    "run_worker_first": [
      "/api/*",           // API routes
      "/admin/*",         // Admin area
      "!/admin/assets/*"  // Except admin assets
    ]
  }
}
```

**Benefits:**
- Reduces Worker invocations
- Lowers costs
- Improves asset delivery performance

### 2. Leverage Navigation Request Optimization

For SPAs, use `compatibility_date = "2025-04-01"` or later:

```jsonc
{
  "compatibility_date": "2025-04-01",
  "assets": {
    "not_found_handling": "single-page-application"
  }
}
```

Navigation requests skip Worker invocation, reducing costs.

### 3. Type Safety with Bindings

Always type your environment:

```typescript
interface Env {
  ASSETS: Fetcher;
}
```

## Common Errors

### "Asset not found"

**Cause:** Asset not in assets directory, wrong path, or assets not deployed
**Solution:** Verify asset exists, check path case-sensitivity, redeploy if needed

### "Worker not invoked for asset"

**Cause:** Asset served directly, `run_worker_first` not configured
**Solution:** Configure `run_worker_first` patterns to include asset routes

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Max asset size | 25 MB | Per file |
| Total assets | 20,000 files | Per deployment |
| Worker invocations | Standard limits | Optimize with routing |
