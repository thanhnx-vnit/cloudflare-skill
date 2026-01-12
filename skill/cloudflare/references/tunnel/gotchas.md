# Tunnel Gotchas

## Common Errors

### "Error 1016 (Origin DNS Error)"

**Cause:** Tunnel not running or not connected
**Solution:**
```bash
cloudflared tunnel info my-tunnel     # Check status
ps aux | grep cloudflared             # Verify running
journalctl -u cloudflared -n 100      # Check logs
```

### "Self-signed certificate rejected"

**Cause:** Origin using self-signed certificate
**Solution:**
```yaml
originRequest:
  noTLSVerify: true      # Dev only
  caPool: /path/to/ca.pem  # Custom CA
```

### "Connection timeout"

**Cause:** Origin slow to respond or timeout settings too low
**Solution:**
```yaml
originRequest:
  connectTimeout: 60s
  tlsTimeout: 20s
  keepAliveTimeout: 120s
```

### "Tunnel not starting"

**Cause:** Invalid config, missing credentials, or tunnel doesn't exist
**Solution:**
```bash
cloudflared tunnel ingress validate  # Validate config
ls -la ~/.cloudflared/*.json         # Verify credentials
cloudflared tunnel list              # Verify tunnel exists
```

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Free tier | Unlimited tunnels | Unlimited traffic |
| Tunnel replicas | 1000 per tunnel | Max concurrent |
| Connection duration | No hard limit | Hours to days |
| Long-lived connections | May drop during updates | WebSocket, SSH, UDP |

## Best Practices

### Security
1. Use remotely-managed tunnels for centralized control
2. Enable Access policies for sensitive services
3. Rotate tunnel credentials regularly
4. Verify TLS certs (`noTLSVerify: false`)
5. Restrict `bastion` service type

### Performance
1. Run multiple replicas for HA
2. Place `cloudflared` close to origin (same network)
3. Use HTTP/2 for gRPC (`http2Origin: true`)
4. Tune keepalive for long-lived connections
5. Monitor connection counts

### Configuration
1. Use environment variables for secrets
2. Version control config files
3. Validate before deploying (`cloudflared tunnel ingress validate`)
4. Test rules (`cloudflared tunnel ingress rule <URL>`)
5. Document rule order (first match wins)

### Operations
1. Monitor tunnel health in dashboard
2. Set up disconnect alerts
3. Graceful shutdown for config updates
4. Keep `cloudflared` updated (1 year support)
5. Use `--no-autoupdate` in prod; control updates manually

## Debug Mode

```bash
cloudflared tunnel --loglevel debug run my-tunnel
cloudflared tunnel ingress rule https://app.example.com
```

## Migration Strategies

### From Ngrok
```yaml
# Ngrok: ngrok http 8000
# Cloudflare Tunnel:
ingress:
  - hostname: app.example.com
    service: http://localhost:8000
  - service: http_status:404
```

### From VPN
```yaml
# Replace VPN with private network routing
warp-routing:
  enabled: true
```

```bash
cloudflared tunnel route ip add 10.0.0.0/8 my-tunnel
```

Users install WARP client instead of VPN.
