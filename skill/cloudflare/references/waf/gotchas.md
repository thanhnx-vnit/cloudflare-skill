### Common Issues & Solutions

## Common Errors

### "False positives blocking legitimate traffic"

**Cause:** WAF rules too aggressive, attack score too low, or missing exceptions
**Solution:** 
- Start with `log` action to monitor
- Use WAF exceptions for specific endpoints
- Override managed ruleset rules to less aggressive actions
- Combine attack score with path filters

### "Rate limiting blocking legitimate users behind NAT"

**Cause:** Multiple users sharing single IP address
**Solution:**
- Use "IP with NAT support" characteristic (Business+)
- Add additional characteristics (headers, cookies)
- Increase rate limits for shared IPs
- Use counting expressions to filter what counts

### "Rules not applying as expected"

**Cause:** Rule order incorrect, syntax error, wrong phase, or conflicting rules
**Solution:**
- Check rule order and priority
- Verify expression syntax with Security Events
- Ensure ruleset is deployed to correct phase
- Check for conflicting skip or allow rules

### "Managed ruleset not working"

**Cause:** Ruleset not deployed or overrides incorrect
**Solution:** Deploy ruleset to correct phase, verify overrides, check rule IDs

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Custom rules | 5 (Free) / 100 (Enterprise) | Per zone |
| Rate limiting rules | Plan dependent | Check docs |
| Rule expression length | 4096 chars | Per rule |
| Rulesets per zone | Varies by plan | Phase dependent |
