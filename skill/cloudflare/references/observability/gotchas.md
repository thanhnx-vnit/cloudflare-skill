## Common Errors

### "Logs not appearing"

**Cause:** Observability disabled, Worker not redeployed, no traffic, low sampling rate, or log size exceeds 256 KB
**Solution:** 
```bash
# Verify config
cat wrangler.jsonc | jq '.observability'

# Check deployment
wrangler deployments list <WORKER_NAME>

# Test with curl
curl https://your-worker.workers.dev
```
Ensure `observability.enabled = true`, redeploy Worker, check `head_sampling_rate`, verify traffic

### "Traces not being captured"

**Cause:** Traces not enabled, incorrect sampling rate, Worker not redeployed, or destination unavailable
**Solution:**
```jsonc
// Temporarily set to 100% sampling for debugging
{
  "observability": {
    "enabled": true,
    "head_sampling_rate": 1.0,
    "traces": {
      "enabled": true
    }
  }
}
```
Ensure `observability.traces.enabled = true`, set `head_sampling_rate` to 1.0 for testing, redeploy, check destination status

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Max log size | 256 KB | Logs exceeding this are truncated |
| Default sampling rate | 0.01 (1%) | Increase for debugging |
| Max destinations | Varies by plan | Check dashboard |
