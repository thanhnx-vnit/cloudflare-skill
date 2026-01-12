# Wrangler Common Issues

## Common Errors

### "Binding ID vs name mismatch"

**Cause:** Confusion between binding name (code) and resource ID
**Solution:** Bindings use `binding` (code name) and `id`/`database_id`/`bucket_name` (resource ID). Preview bindings need separate IDs: `preview_id`, `preview_database_id`

### "Environment not inheriting config"

**Cause:** Non-inheritable keys not redefined per environment
**Solution:** Non-inheritable keys (bindings, vars) must be redefined per environment. Inheritable keys (routes, compatibility_date) can be overridden

### "Local dev behavior differs from production"

**Cause:** Using local simulation instead of remote execution
**Solution:** `wrangler dev` (default): Local simulation, fast, limited accuracy. `wrangler dev --remote`: Remote execution, slower, production-accurate

### "Unexpected runtime changes"

**Cause:** Missing compatibility_date
**Solution:** Always set `compatibility_date`:
```jsonc
{ "compatibility_date": "2025-01-01" }
```

### "Durable Object binding not working"

**Cause:** Missing script_name in getPlatformProxy
**Solution:** Always specify `script_name`:
```jsonc
{
  "durable_objects": {
    "bindings": [
      { "name": "MY_DO", "class_name": "MyDO", "script_name": "my-worker" }
    ]
  }
}
```

### "Secrets not available in local dev"

**Cause:** Secrets set with `wrangler secret put` only work in deployed Workers
**Solution:** For local dev, use `.dev.vars`

### "Node.js compatibility error"

**Cause:** Missing Node.js compatibility flag
**Solution:** Some bindings (Hyperdrive with `pg`) require:
```jsonc
{ "compatibility_flags": ["nodejs_compat_v2"] }
```

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Bindings per Worker | 64 | Check wrangler.jsonc |
| Environments | Unlimited | Configure per env |
| Config file size | Reasonable | Keep under 1MB |

## Troubleshooting

### Authentication Issues
```bash
wrangler logout
wrangler login
wrangler whoami
```

### Configuration Errors
```bash
wrangler check  # Validate config
```
Use wrangler.jsonc with `$schema` for validation.

### Binding Not Available
- Check binding exists in config
- For environments, ensure binding defined for that env
- Local dev: some bindings need `--remote`

### Deployment Failures
```bash
wrangler tail              # Check logs
wrangler deploy --dry-run  # Validate
wrangler whoami            # Check account limits
```

### Local Development Issues
```bash
rm -rf .wrangler/state     # Clear local state
wrangler dev --remote      # Use remote bindings
wrangler dev --persist-to ./local-state  # Custom persist location
```

## Resources

- Docs: https://developers.cloudflare.com/workers/wrangler/
- Config: https://developers.cloudflare.com/workers/wrangler/configuration/
- Commands: https://developers.cloudflare.com/workers/wrangler/commands/
- Examples: https://github.com/cloudflare/workers-sdk/tree/main/templates
- Discord: https://discord.gg/cloudflaredev

## See Also

- [README.md](./README.md) - Commands
- [configuration.md](./configuration.md) - Config
- [api.md](./api.md) - Programmatic API
- [patterns.md](./patterns.md) - Workflows
