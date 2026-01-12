## Troubleshooting

### Binding Not Found

**Common Errors:**

### "env.MY_KV is undefined"

**Cause:** Binding name mismatch, binding not configured, or types not regenerated
**Solution:**
1. Check binding name matches config
2. Run `npx wrangler types` to regenerate types
3. Verify binding exists in current environment
4. Check for typos in `wrangler.jsonc`

### "Property 'MY_KV' does not exist on type 'Env'"

**Cause:** TypeScript types not generated or out of sync
**Solution:** Run `npx wrangler types`

### "preview_id is required for --remote"

**Cause:** Missing preview binding configuration
**Solution:** Add preview_id to config or use local mode

### "Secret updated but Worker still uses old value"

**Cause:** Environment values cached in global scope
**Solution:** Avoid caching `env` values in global scope

## Security Best Practices

1. **Never commit secrets to config files**
   - Use `npx wrangler secret put` for sensitive values
   - Secrets are encrypted

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Bindings per Worker | 64 | All types combined |
| Environment variables | 64 max | 5 KB per var |
| Secret size | 1024 bytes | Per secret |
