# DDoS Gotchas

## Common Errors

### "False positives blocking legitimate traffic"

**Cause:** Sensitivity too high, wrong action, or missing exceptions
**Solution:**
```typescript
// Query GraphQL API for flagged requests
const query = `
  query {
    viewer {
      zones(filter: { zoneTag: "${zoneId}" }) {
        httpRequestsAdaptiveGroups(
          filter: { ruleId: "${ruleId}", action: "log" }
          limit: 100
          orderBy: [datetime_DESC]
        ) {
          dimensions {
            clientCountryName
            clientRequestHTTPHost
            clientRequestPath
            userAgent
          }
          count
        }
      }
    }
  }
`;
```

1. Lower sensitivity for specific rule/category
2. Use `log` action first to validate (Enterprise Advanced)
3. Add exception with custom expression (e.g., allowlist IPs)
4. Or allowlist by IP/ASN/country

### "Attacks getting through"

**Cause:** Sensitivity too low or wrong action
**Solution:**
```typescript
// Increase to default (high) sensitivity
const config = {
  rules: [{
    expression: "true",
    action: "execute",
    action_parameters: {
      id: managedRulesetId,
      overrides: { sensitivity_level: "default", action: "block" },
    },
  }],
};
```

### "Adaptive rules not working"

**Cause:** Insufficient traffic history (needs 7 days)
**Solution:** Wait for baseline to establish, check dashboard for adaptive rule status

### "Zone override ignored"

**Cause:** Account overrides conflict with zone overrides
**Solution:** Configure at zone level OR remove zone overrides to use account-level

### "Log action not available"

**Cause:** Not on Enterprise Advanced DDoS plan
**Solution:** Use `managed_challenge` with low sensitivity for testing

### "Rule limit exceeded"

**Cause:** Too many override rules (Free/Pro/Business: 1, Enterprise Advanced: 10)
**Solution:** Combine conditions in single expression using `and`/`or`

### "Cannot override rule"

**Cause:** Rule is read-only
**Solution:** Check API response for read-only indicator, use different rule

### "Cannot disable DDoS protection"

**Cause:** DDoS managed rulesets cannot be fully disabled (always-on protection)
**Solution:** Set `sensitivity_level: "eoff"` for minimal mitigation

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Override rules (Free/Pro/Business) | 1 | Per zone |
| Override rules (Enterprise Advanced) | 10 | Per zone |
| Traffic history for adaptive | 7 days | Required |
| Sensitivity levels | eoff, low, medium, default, high | Configurable |

## Tuning Strategy

1. Start with `log` action + `medium` sensitivity
2. Monitor for 24-48 hours
3. Identify false positives, add exceptions
4. Gradually increase to `default` sensitivity
5. Change action from `log` → `managed_challenge` → `block`
6. Document all adjustments

## Best Practices

- Test during low-traffic periods
- Use zone-level for per-site tuning
- Reference IP lists for easier management
- Set appropriate alert thresholds (avoid noise)
- Combine with WAF for layered defense
- Avoid over-tuning (keep config simple)

See [patterns.md](./patterns.md) for progressive rollout examples.
