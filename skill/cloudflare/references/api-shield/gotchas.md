# Gotchas & Troubleshooting

## Common Errors

### "Schema validation blocking valid requests"

**Cause:** Schema too restrictive, missing fields, or incorrect types
**Solution:** 
1. Check Firewall Events for violation details
2. Review schema in Settings
3. Test in Swagger Editor
4. Use log mode to validate
5. Update schema with correct specifications

### "JWT validation failing"

**Cause:** JWKS mismatch with IdP, expired token, wrong header/cookie name, or clock skew
**Solution:** 
1. Verify JWKS matches IdP configuration
2. Check token `exp` claim is valid
3. Confirm header/cookie name matches config
4. Test token at jwt.io
5. Account for clock skew

### "Endpoint discovery not finding APIs"

**Cause:** Insufficient traffic (<500 reqs/10d), non-2xx responses, Worker direct requests, or incorrect session ID config
**Solution:** Ensure 500+ requests in 10 days, 2xx responses from edge (not Workers direct), configure session IDs correctly. ML updates daily.

### "Sequence detection false positives"

**Cause:** Lookback window issues, non-unique session IDs, or model sensitivity
**Solution:** Review lookback settings (10 reqs to managed endpoints, 10min window), ensure session ID uniqueness per user, adjust positive/negative model balance

### "Token invalid"

**Cause:** Configuration error, JWKS mismatch, or expired token
**Solution:** Verify config matches IdP, update JWKS, check token expiration

### "Schema violation"

**Cause:** Missing required fields, wrong data types, or spec mismatch
**Solution:** Review schema against actual requests, ensure all required fields present, validate types match spec

### "Fallthrough"

**Cause:** Unknown endpoint or pattern mismatch
**Solution:** Update schema with all endpoints, check path pattern matching

### "mTLS failed"

**Cause:** Certificate untrusted/expired or wrong CA
**Solution:** Verify cert chain, check expiration, confirm correct CA uploaded

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| OpenAPI version | v3.0.x only | No external refs |
| Schema operations | 10K max | Must have `type` + `schema` |
| Validation sources | Headers/cookies only | For JWT |
| Endpoint discovery | 500+ reqs/10d | Minimum for ML |
| Path normalization | Automatic | `/profile/238` → `/profile/{var1}` |
| Schema parameters | No `content` | No object param validation |
| JWT managed endpoints | Validated only | Not all requests |
| Sequence detection | Beta | Contact team to enable |
