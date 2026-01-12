# Gotchas & Best Practices

## Common Errors

### "setState() not syncing"

**Cause:** Mutating state directly or not calling `setState()` after modifications
**Solution:** Always use `setState()` with immutable updates:
```ts
// ❌ this.state.count++
// ✅ this.setState({...this.state, count: this.state.count + 1})
```

### "SQL injection vulnerability"

**Cause:** Direct string interpolation in SQL queries
**Solution:** Use parameterized queries:
```ts
// ❌ this.sql`...WHERE id = '${userId}'`
// ✅ this.sql`...WHERE id = ${userId}`
```

### "WebSocket connection timeout"

**Cause:** Not calling `conn.accept()` in `onConnect`
**Solution:** Always accept connections:
```ts
async onConnect(conn: Connection, ctx: ConnectionContext) { conn.accept(); conn.setState({userId: "123"}); }
```

### "Schedule limit exceeded"

**Cause:** More than 1000 scheduled tasks per agent
**Solution:** Clean up old schedules and limit creation rate:
```ts
async checkSchedules() { if ((await this.getSchedules()).length > 800) console.warn("Near limit!"); }
```

### "AI Gateway unavailable"

**Cause:** AI service timeout or quota exceeded
**Solution:** Add error handling and fallbacks:
```ts
try { return await this.env.AI.run(model, {prompt}); } catch (e) { return {error: "Unavailable"}; }
```

### "Agent not found"

**Cause:** Durable Object binding missing or incorrect class name
**Solution:** Verify DO binding in wrangler.jsonc and class name matches

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| CPU per request | 30s | Default, max 300s |
| Memory per instance | 128MB | Shared with WebSockets |
| Storage per agent | Shares DO quota | 10GB max for SQLite |
| Scheduled tasks | 1000 per agent | Clean up old tasks |
| WebSocket connections | Unlimited | Within memory limits |
| SQL columns | 100 | Per table |
| SQL row size | 2MB | Key + value |
| WebSocket message | 32MiB | Max size |
