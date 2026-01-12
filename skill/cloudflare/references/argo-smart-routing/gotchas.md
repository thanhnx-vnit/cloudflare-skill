## Best Practices Summary

1. **Always check editability** before attempting to enable/disable Argo
2. **Set up billing notifications** to avoid unexpected costs
3. **Combine with Tiered Cache** for maximum performance benefit
4. **Use in production only** - disable for dev/staging to control costs
5. **Monitor analytics** - require 500+ requests in 48h for detailed metrics
6. **Handle errors gracefully** - check for billing, permissions, zone compatibility
7. **Test configuration changes** in staging before production
8. **Use TypeScript SDK** for type safety and better developer experience
9. **Implement retry logic** for API calls in production systems
10. **Document zone-specific settings** for team visibility

## Common Errors

### "Argo unavailable"

**Cause:** Zone not eligible or billing not set up
**Solution:** Check zone plan, verify billing configured, ensure zone meets eligibility requirements

### "Cannot enable/disable"

**Cause:** Insufficient permissions or zone restrictions
**Solution:** Verify API token has edit permissions, check zone editability status

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Min requests for analytics | 500 in 48h | For detailed metrics |
| Zones supported | Enterprise+ | Check plan |
| Billing requirement | Must be configured | Before enabling |

## Additional Resources

- [Official Argo Smart Routing Docs](https://developers.cloudflare.com/argo-smart-routing/)
