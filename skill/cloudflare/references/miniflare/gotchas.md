# Gotchas & Debugging

## Compatibility Issues

### Not Supported in Miniflare

- Cloudflare Analytics Engine
- Cloudflare Images
- Live production data
- True global distribution
- Some advanced Workers features

### Behavior Differences from Production

1. **No actual edge:** Runs in workerd locally, not Cloudflare's global network
2. **Persistence:** Local filesystem/in-memory, not distributed
3. **Request.cf:** Fetched from cached endpoint or mocked, not real edge metadata
4. **Performance:** Local performance ≠ edge performance
5. **Caching:** May differ slightly from production

## Common Errors

### "Cannot find module"

**Cause:** Module path incorrect or modulesRules not configured
**Solution:**
```js
// Use absolute paths or modulesRules
new Miniflare({
  scriptPath: "./src/index.js",
  modules: true,
  modulesRules: [
    { type: "ESModule", include: ["**/*.js"], fallthrough: true },
  ],
});
```

### "Data not persisting between runs"

**Cause:** Persist paths are files instead of directories
**Solution:**
```js
// Ensure persist paths are directories, not files
new Miniflare({
  kvPersist: "./data/kv",           // Directory
  r2Persist: "./data/r2",
  durableObjectsPersist: "./data/do",
});
```

### "Cannot directly run TypeScript"

**Cause:** Miniflare doesn't transpile TypeScript
**Solution:**
```js
// Build before running
import { spawnSync } from "node:child_process";

before(() => {
  const result = spawnSync("npm run build", { shell: true });
  if (result.error) throw result.error;
});

new Miniflare({ scriptPath: "dist/worker.js" });
```

### "`request.cf` is undefined"

**Cause:** CF object not configured
**Solution:**
```js
new Miniflare({
  cf: true, // Fetch from Cloudflare
  // Or provide custom
  cf: "./cf.json",
});
```

### "EADDRINUSE error"

**Cause:** Port already in use by another process
**Solution:**
```js
// Don't specify port for testing - use dispatchFetch
new Miniflare({
  scriptPath: "worker.js",
  // No port/host
});

const res = await mf.dispatchFetch("http://localhost/");
```

### "Durable Object not defined"

**Cause:** DO class not exported or name mismatch
**Solution:**
```js
// Ensure DO class is exported
new Miniflare({
  modules: true, // Required for DOs
  script: `
    export class Counter { /* ... */ } // Must export
    export default { /* ... */ }
  `,
  durableObjects: {
    COUNTER: "Counter", // Must match export name
  },
});
```

## Debugging Tips

**Enable debug logging:**
```js
import { Log, LogLevel } from "miniflare";
new Miniflare({ log: new Log(LogLevel.DEBUG) });
```

**Check binding names match:**
```js
const bindings = await mf.getBindings();
console.log(Object.keys(bindings));
```

**Verify storage directly:**
```js
const ns = await mf.getKVNamespace("TEST");
const keys = await ns.list();
console.log(keys);
```

**Test HTTP server separately:**
```js
// Use dispatchFetch for tests, not HTTP server
const res = await mf.dispatchFetch("http://localhost/");
```

## Migration Notes

### From Wrangler Dev to Miniflare

**Wrangler:**
```bash
wrangler dev
```

**Miniflare:**
```js
new Miniflare({
  scriptPath: "dist/worker.js",
  // Manually configure bindings (doesn't read wrangler.jsonc)
  kvNamespaces: ["KV"],
  bindings: { API_KEY: "..." },
});
```

**Note:** Miniflare doesn't read `wrangler.jsonc` - configure everything via API.

### From Miniflare 2 to 3

Major changes:
- Different API surface
- Better workerd integration
- Changed persistence options
- See [official migration guide](https://developers.cloudflare.com/workers/testing/vitest-integration/migration-guides/migrate-from-miniflare-2/)

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Local storage | Filesystem based | Performance varies |
| Concurrent Workers | System dependent | Memory limited |
| Request.cf data | Cached or mocked | Not real edge data |

## When to Use

**Use Miniflare when:**
- Writing integration tests for Workers
- Testing Worker bindings/storage locally
- Testing multiple Workers with service bindings
- Need direct access to bindings in tests
- Dispatch events without HTTP

**Use Wrangler instead for:**
- Standard development workflow
- Quick local dev server
- Production deployments

See [patterns.md](./patterns.md) for testing examples.
