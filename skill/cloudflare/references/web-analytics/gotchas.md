## Troubleshooting

## Common Errors

### "Beacon Not Loading"

**Cause:** `Cache-Control: no-transform` header, beacon placement issue, or CSP blocking script
**Solution:**
1. Check `Cache-Control` header - remove `no-transform` if present
2. Verify beacon placement before `</body>`
3. Check CSP (Content Security Policy) allows `static.cloudflareinsights.com`
4. For proxied sites: Ensure auto-injection enabled in dashboard

**CSP Fix:**
```html
<meta http-equiv="Content-Security-Policy" 
      content="script-src 'self' 'unsafe-inline' https://static.cloudflareinsights.com;">
```

### "No Data Appearing"

**Cause:** Data ingestion delay, incorrect token, beacon errors, no traffic, or beacon not loaded
**Solution:**
1. Wait 5-10 minutes after setup (data ingestion delay)
2. Verify token is correct in `data-cf-beacon`
3. Check browser console for beacon errors
4. Confirm site has real traffic (test by visiting pages)
5. Verify beacon script loading

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Data delay | 5-10 minutes | After beacon fires |
| Token format | Alphanumeric | From dashboard |
| Sites per account | Unlimited | Free tier |
