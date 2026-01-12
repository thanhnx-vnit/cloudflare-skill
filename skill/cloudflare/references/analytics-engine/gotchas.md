# Gotchas

See main documentation.

## Common Errors

### "Dataset binding not found"

**Cause:** Binding not configured in wrangler.jsonc or name mismatch
**Solution:** Add Analytics Engine binding to wrangler.jsonc, verify binding name matches code

### "Write failed"

**Cause:** Invalid data format, exceeded rate limit, or quota exhausted
**Solution:** Verify data format correct (blobs as strings, doubles as numbers), check rate limits, monitor quota usage

### "Query returned no data"

**Cause:** No data points written, query filter too restrictive, or time range invalid
**Solution:** Verify data written successfully, broaden query filters, check time range

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Max data points per request | 25 | Per writeDataPoint call |
| Max blobs per data point | 20 | String indexes |
| Max doubles per data point | 20 | Numeric values |
| Blob size | 5,096 bytes | Per blob |
| Index cardinality | High | Use wisely |
| Data retention | 90 days | Default |
