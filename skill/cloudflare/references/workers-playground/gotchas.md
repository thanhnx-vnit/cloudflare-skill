## Best Practices

### 1. Use Async/Await
Always use `async/await` for cleaner asynchronous code:

```javascript
// Good
export default {
  async fetch(request, env, ctx) {
    const response = await fetch('https://api.example.com');
    const data = await response.json();
    return new Response(JSON.stringify(data));
  },
};

// Avoid
export default {
  fetch(request, env, ctx) {
    return fetch('https://api.example.com')
      .then(response => response.json())
      .then(data => new Response(JSON.stringify(data)));
  },
};
```

### 2. Clone Responses Before Reading
Response bodies can only be read once:

```javascript
export default {
  async fetch(request, env, ctx) {
    const response = await fetch('https://api.example.com');
    
    // Clone before caching
    ctx.waitUntil(caches.default.put(request, response.clone()));
    
    return response;
  },
};
```

## Common Errors

### "Response body already read"

**Cause:** Attempting to read response body twice
**Solution:** Clone response before reading: `response.clone()`

### "Worker exceeded CPU time"

**Cause:** Code execution took too long
**Solution:** Optimize code, use `ctx.waitUntil()` for background tasks

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| CPU time | 10ms (Free) / 50ms (Paid) | Per request |
| Script size | 1 MB | Compressed |
| Subrequests | 1000 | Per request |
