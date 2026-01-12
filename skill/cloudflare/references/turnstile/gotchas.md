## Troubleshooting

## Common Errors

### "Widget not rendering"

**Cause:** Incorrect sitekey, JavaScript disabled, CSP blocking script, or file:// protocol
**Solution:** 
- Check browser console for errors
- Verify sitekey is correct
- Ensure JavaScript is enabled
- Check for CSP (Content Security Policy) blocking script
- Verify not using `file://` protocol (only `http://` and `https://` work)

### "Validation failing"

**Cause:** Secret key incorrect, token expired (>5 min), token already validated, network issues, or test key in production
**Solution:**
- Check secret key is correct
- Verify token not expired (>5 min old)
- Ensure token not already validated (single-use)
- Check server can reach Siteverify API
- Verify not using test secret key in production

### CSP Configuration
```html
<meta http-equiv="Content-Security-Policy" 
      content="script-src 'self' https://challenges.cloudflare.com; 
               frame-src https://challenges.cloudflare.com;">
```

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Token validity | 5 minutes | After generation |
| Token use | Single-use | Cannot revalidate |
| API rate limits | Varies by plan | Check docs |

## Reference

### Official Docs
- [Turnstile Overview](https://developers.cloudflare.com/turnstile/)
