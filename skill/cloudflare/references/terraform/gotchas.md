# Terraform Troubleshooting & Best Practices

Common issues, security considerations, and best practices.

## Common Errors

### "Error: couldn't find resource"

**Cause:** Resource was deleted outside Terraform  
**Solution:** Import resource back into state with `terraform import cloudflare_zone.example <zone-id>` or remove from state with `terraform state rm cloudflare_zone.example`

### "409 Conflict on worker deployment"

**Cause:** Worker being deployed by both Terraform and wrangler simultaneously  
**Solution:** Choose one deployment method; if using Terraform, remove wrangler deployments

### "DNS record already exists"

**Cause:** Existing DNS record not imported into Terraform state  
**Solution:** Find record ID in Cloudflare dashboard and import with `terraform import cloudflare_dns_record.example <zone-id>/<record-id>`

### "Invalid provider configuration"

**Cause:** API token missing, invalid, or lacking required permissions  
**Solution:** Set `CLOUDFLARE_API_TOKEN` environment variable or check token permissions in dashboard

### "State locking errors"

**Cause:** Multiple concurrent Terraform runs or stale lock from crashed process  
**Solution:** Remove stale lock with `terraform force-unlock <lock-id>` (use with caution)

## Limits

| Resource | Limit | Notes |
|----------|-------|-------|
| API token rate limit | Varies by plan | Use `api_client_logging = true` to debug
| Worker script size | 10 MB | Includes all dependencies
| KV keys per namespace | Unlimited | Pay per operation
| R2 storage | Unlimited | Pay per GB
| D1 databases | 50,000 per account | Free tier: 10
| Pages projects | 500 per account | 100 for free accounts
| DNS records | 3,500 per zone | Free plan

## See Also

- [README](./README.md) - Provider setup
- [Configuration](./configuration.md) - Resources
- [API](./api.md) - Data sources
- [Patterns](./patterns.md) - Use cases
- Provider docs: https://registry.terraform.io/providers/cloudflare/cloudflare/latest/docs
