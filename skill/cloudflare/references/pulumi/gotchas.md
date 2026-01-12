# Troubleshooting & Best Practices

## Common Errors

### "Missing required property 'accountId'"

**Cause:** Account ID not provided in resource configuration  
**Solution:** Add accountId to config or pass explicitly in resource definition

### "Binding Name Mismatch"

**Cause:** Worker code expects binding with different name than configured in Pulumi  
**Solution:** Match binding names in Pulumi configuration and worker code

### "resource 'abc123' not found"

**Cause:** Resource doesn't exist in specified account/zone or has been deleted  
**Solution:** Ensure resources exist in correct account/zone and verify resource IDs

### "API Token Permissions Insufficient"

**Cause:** API token lacks required permissions for operation  
**Solution:** Verify token has required permissions (Workers, KV, R2, D1, DNS, Pages, etc.) in Cloudflare dashboard



## Limits

| Limit | Value | Notes |
|-------|-------|-------|
| API token rate limit | Varies by plan | Use `api_client_logging = true` to debug |
| Worker script size | 10 MB | Includes all dependencies |
| KV keys per namespace | Unlimited | Pay per operation |
| R2 storage | Unlimited | Pay per GB |
| D1 databases | 50,000 per account | Free tier: 10 |
| Pages projects | 500 per account | Free: 100 |
| DNS records | 3,500 per zone | Free plan |

## Resources

- **Pulumi Registry:** https://www.pulumi.com/registry/packages/cloudflare/
- **API Docs:** https://www.pulumi.com/registry/packages/cloudflare/api-docs/
- **GitHub:** https://github.com/pulumi/pulumi-cloudflare
- **Cloudflare Docs:** https://developers.cloudflare.com/
- **Workers Docs:** https://developers.cloudflare.com/workers/

---
See: [README.md](./README.md), [configuration.md](./configuration.md), [api.md](./api.md), [patterns.md](./patterns.md)
