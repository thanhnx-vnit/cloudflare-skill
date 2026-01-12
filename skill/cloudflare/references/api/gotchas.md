## Best Practices

### Security

- **Never commit tokens**: Use environment variables
- **Minimal permissions**: Create scoped API tokens
- **Rotate tokens**: Regularly refresh tokens
- **Use token expiration**: Set expiry dates
- **Audit token usage**: Monitor API logs

### Performance

- **Batch operations**: Group related API calls
- **Use pagination wisely**: Don't fetch all data if unnecessary
- **Cache responses**: Store rarely-changing data locally
- **Parallel requests**: Use `Promise.all()` for independent operations
- **Handle rate limits**: Implement exponential backoff

### Code organization

```typescript
// Create reusable client instance
export const cfClient = new Cloudflare({
  apiToken: process.env.CLOUDFLARE_API_TOKEN,
});

// Wrap common operations
export async function getZoneDetails(zoneId: string) {
  return await cfClient.zones.get(zoneId);
}
```

## Common Errors

### "Authentication failed"

**Cause:** Invalid API token, expired token, or insufficient permissions
**Solution:** Verify API token valid, check expiration, ensure token has required permissions

### "Rate limit exceeded"

**Cause:** Too many requests in short period
**Solution:** Implement exponential backoff, reduce request frequency, use caching

### "Resource not found"

**Cause:** Incorrect resource ID, resource deleted, or wrong account/zone
**Solution:** Verify resource ID correct, check resource exists, confirm correct account context

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| API rate limits | Varies by endpoint | Check response headers |
| Token permissions | Scoped | Create with minimal access |
| Parallel requests | Recommended < 10 | Avoid overwhelming API |
