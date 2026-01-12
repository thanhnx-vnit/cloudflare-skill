# Cache Reserve Gotchas

## Common Errors

### "Assets Not Being Cached in Cache Reserve"

**Cause:** Asset is not cacheable, TTL < 10 hours, Content-Length header missing, or blocking headers present (Set-Cookie, Vary: *)  
**Solution:** Ensure minimum TTL of 10+ hours (`Cache-Control: public, max-age=36000`), add Content-Length header, remove Set-Cookie header, and set `Vary: Accept-Encoding` (not *)

### "High Class A Operations Costs"

**Cause:** Frequent cache misses, short TTLs, or frequent revalidation  
**Solution:** Increase TTL for stable content (24+ hours), enable Tiered Cache to reduce direct Cache Reserve misses, or use stale-while-revalidate

### "Purge Not Working as Expected"

**Cause:** Purge by tag only triggers revalidation but doesn't remove from Cache Reserve storage  
**Solution:** Use purge by URL for immediate removal, or disable Cache Reserve then clear all data for complete removal

### "Cache Reserve must be OFF before clearing data"

**Cause:** Attempting to clear Cache Reserve data while it's still enabled  
**Solution:** Disable Cache Reserve first, wait briefly for propagation (5s), then clear data (can take up to 24 hours)

## Limits

| Limit | Value | Notes |
|-------|-------|-------|
| Minimum TTL | 10 hours (36000 seconds) | Assets with shorter TTL not eligible |
| Default retention | 30 days (2592000 seconds) | Configurable |
| Maximum file size | Same as R2 limits | No practical limit |
| Purge/clear time | Up to 24 hours | Complete propagation time |
| Plan requirement | Paid Cache Reserve plan | Not available on free plans |
| Content-Length header | Required | Must be present for eligibility |
| Set-Cookie header | Blocks caching | Must not be present (or use private directive) |
| Vary header | Cannot be * | Can use Vary: Accept-Encoding |
| Image transformations | Variants not eligible | Original images only |

## Additional Resources

- **Official Docs**: https://developers.cloudflare.com/cache/advanced-configuration/cache-reserve/
- **API Reference**: https://developers.cloudflare.com/api/resources/cache/subresources/cache_reserve/
- **Cache Rules**: https://developers.cloudflare.com/cache/how-to/cache-rules/
- **Workers Cache API**: https://developers.cloudflare.com/workers/runtime-apis/cache/
- **R2 Documentation**: https://developers.cloudflare.com/r2/
- **Smart Shield**: https://developers.cloudflare.com/smart-shield/
- **Tiered Cache**: https://developers.cloudflare.com/cache/how-to/tiered-cache/

## See Also

- [README](./README.md) - Overview and core concepts
- [Configuration](./configuration.md) - Setup and Cache Rules
- [API Reference](./api.md) - Purging and monitoring
- [Patterns](./patterns.md) - Best practices and optimization
