## Best Practices

### Performance
1. **Keep code minimal**: 32KB limit, but aim for <10KB
2. **Avoid blocking operations**: 5ms execution limit
3. **Clone only when needed**: `new Request(request)` creates copy
4. **Cache strategically**: Use `caches.default` for repeated data
5. **Limit subrequests**: Plan-based limits (2-5)

### Security
1. **Validate input**: Never trust user data
2. **Use Web Crypto API**: For cryptographic operations
3. **Sanitize headers**: Remove sensitive information
4. **Check bot scores**: Use `request.cf.botManagement.score`
5. **Rate limit carefully**: Snippets run on every matching request

### Debugging
1. **Test in dashboard**: Use HTTP/Preview tabs
2. **Start simple**: Test with basic logic first
3. **Use custom headers**: Add debug headers to responses

## Common Errors

### "Snippet execution failed"

**Cause:** Syntax error, exceeded execution limit, or unhandled exception
**Solution:** Validate syntax, optimize code for performance, add error handling

### "Request exceeded execution limit"

**Cause:** Code took longer than 5ms to execute
**Solution:** Optimize hot paths, reduce complexity, move heavy operations to Workers

### "Subrequest limit exceeded"

**Cause:** Too many fetch calls within snippet
**Solution:** Reduce number of subrequests, batch operations, cache results

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Snippet size | 32 KB | Compressed |
| Execution time | 5ms | Per request |
| Subrequests | 2-5 | Plan dependent |
| Memory | Shared with zone | Limited |
