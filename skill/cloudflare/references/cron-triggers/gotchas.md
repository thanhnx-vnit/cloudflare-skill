# Cron Triggers Gotchas

## Common Errors

### "Timezone Issues"

**Cause:** Cron triggers only support UTC timezone, no local timezone configuration  
**Solution:** Convert local time to UTC using formula: `utcHour = (localHour - utcOffset + 24) % 24`. Example: 9am PST (UTC-8) = 17:00 UTC, so use "0 17 * * *"

### "Cron Not Executing"

**Cause:** Missing `scheduled()` export, no recent deploy, waiting less than 15min for propagation, invalid cron syntax, or plan limits exceeded  
**Solution:** Verify `scheduled()` is exported, deploy recently, wait 15+ minutes, validate cron at crontab.guru, and check plan trigger limits

### "Duplicate Executions"

**Cause:** At-least-once delivery guarantees can result in duplicate executions  
**Solution:** Make handlers idempotent by tracking execution IDs in KV with TTL

### "Execution Failures"

**Cause:** CPU time exceeded, unhandled exceptions, network timeouts, or binding misconfiguration  
**Solution:** Wrap in try-catch, use AbortController for network timeouts, use `ctx.waitUntil()` for long operations, or use Workflows for heavy processing

### "Local Testing Not Working"

**Cause:** Wrangler not running, `scheduled()` not defined, or outdated Wrangler version  
**Solution:** Ensure `wrangler dev` runs, `scheduled()` exists, update Wrangler with `npm i -g wrangler@latest`, and use `curl "http://localhost:8787/__scheduled"` to test

### "Security Concerns"

**Cause:** Hardcoded secrets in code or `__scheduled` endpoint exposed in production  
**Solution:** Use `env.API_KEY` for secrets (not hardcoded strings) and block `__scheduled` endpoint in production environment

## Limits

| Limit | Free | Paid | Notes |
|-------|------|------|-------|
| Triggers per Worker | 3 | Unlimited | Maximum cron schedules per Worker |
| CPU time | 10ms | 50ms | May need `ctx.waitUntil()` or Workflows |
| Execution guarantee | At-least-once | At-least-once | Duplicates possible |
| Propagation delay | Up to 15 minutes | Up to 15 minutes | Time for changes to take effect |

## Resources

- [Cron Triggers Docs](https://developers.cloudflare.com/workers/configuration/cron-triggers/)
- [Scheduled Handler API](https://developers.cloudflare.com/workers/runtime-apis/handlers/scheduled/)
- [Cloudflare Workflows](https://developers.cloudflare.com/workflows/)
- [Workers Limits](https://developers.cloudflare.com/workers/platform/limits/)
- [Crontab Guru](https://crontab.guru/) - Validator
