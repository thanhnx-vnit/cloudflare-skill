## Best Practices

### Always Close Browsers
```typescript
// ❌ BAD - Session stays open until timeout
const browser = await puppeteer.launch(env.MYBROWSER);
const page = await browser.newPage();
await page.goto("https://example.com");
return new Response(await page.content());

// ✅ GOOD - Always use try/finally
const browser = await puppeteer.launch(env.MYBROWSER);
try {
  const page = await browser.newPage();
  await page.goto("https://example.com");
  return new Response(await page.content());
} finally {
  await browser.close(); // Ensures cleanup even on errors
}
```

### Optimize Concurrency
Instead of launching multiple browsers:
- Use multiple tabs in single browser
- Reuse sessions with session IDs
- Use incognito contexts for isolation without new browsers

```typescript
// ❌ BAD: Multiple browsers
const browser1 = await puppeteer.launch(env.MYBROWSER);
const browser2 = await puppeteer.launch(env.MYBROWSER);

// ✅ GOOD: One browser, multiple pages
const browser = await puppeteer.launch(env.MYBROWSER);
const page1 = await browser.newPage();
const page2 = await browser.newPage();
```

## Common Errors

### "Browser session leaked"

**Cause:** Browser not closed properly
**Solution:** Always use try/finally to ensure `browser.close()` called

### "Session limit exceeded"

**Cause:** Too many concurrent browser sessions
**Solution:** Close unused browsers, reuse sessions, optimize concurrency

### "Page navigation timeout"

**Cause:** Page took too long to load
**Solution:** Increase timeout, optimize target page, check network

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Concurrent sessions | Varies by plan | Close unused |
| Page load timeout | 30s default | Configurable |
| Memory per session | ~50-100MB | Monitor usage |
