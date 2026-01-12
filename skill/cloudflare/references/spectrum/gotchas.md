## Troubleshooting

## Common Errors

### "Connection timeouts"

**Cause:** Origin firewall blocking Cloudflare IPs, origin service not running, or incorrect DNS configuration
**Solution:**
```bash
# Test connectivity to Spectrum app
nc -zv app.example.com 22

# Check DNS resolution
dig app.example.com

# Verify origin accepts Cloudflare IPs
curl -v telnet://origin-ip:port
```
Verify origin firewall allows Cloudflare IPs, check origin service running on correct port, ensure DNS record is CNAME

### "Client IP showing Cloudflare IP"

**Cause:** Proxy Protocol not enabled or not configured on origin
**Solution:** Enable Proxy Protocol and configure origin to parse headers:
```json
{
  "proxy_protocol": "v1"  // TCP: v1 or v2; UDP: simple
}
```
Ensure origin application parses proxy protocol headers

### "TLS errors"

**Cause:** Certificate mismatch or TLS configuration incorrect
**Solution:**
```bash
openssl s_client -connect app.example.com:443 -showcerts
```
Use `tls: "flexible"` for testing or configure proper certificates

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Max apps per zone | Varies by plan | Check dashboard |
| Supported protocols | TCP, UDP | Port range dependent |
| Proxy Protocol | v1, v2, simple | TCP and UDP |
