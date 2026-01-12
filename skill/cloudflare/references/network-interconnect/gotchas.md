# CNI Gotchas & Troubleshooting

## Common Errors

### "Status: Pending"

**Cause:** Cross-connect not installed, RX/TX fibers reversed, wrong fiber type, or low light levels
**Solution:**
1. Verify cross-connect installed
2. Check fiber at patch panel
3. Swap RX/TX fibers
4. Check light with optical power meter (target > -20 dBm)
5. Contact account team

### "Status: Unhealthy"

**Cause:** Physical issue, low light (<-20 dBm), optic mismatch, or dirty connectors
**Solution:**
1. Check physical connections
2. Clean fiber connectors
3. Verify optic types (10GBASE-LR/100GBASE-LR4)
4. Test with known-good optics
5. Check patch panel
6. Contact account team

### "BGP Session Down"

**Cause:** Wrong IP addressing, wrong ASN, password mismatch, or firewall blocking TCP/179
**Solution:**
1. Verify IPs match CNI object
2. Confirm ASN correct
3. Check BGP password
4. Verify no firewall on TCP/179
5. Check BGP logs
6. Review BGP timers

### "Low Throughput"

**Cause:** MTU mismatch, fragmentation, single GRE tunnel (v1), or routing inefficiency
**Solution:**
1. Check MTU (1500↓/1476↑ for v1, 1500 both for v2)
2. Test various packet sizes
3. Add more GRE tunnels (v1)
4. Consider upgrading to v2
5. Review routing tables
6. Use LACP for bundling (v1)

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Max optical distance | 10km | Physical limit |
| MTU (v1) | 1500↓ / 1476↑ | Asymmetric |
| MTU (v2) | 1500 both | Symmetric |
| GRE tunnel throughput | 1 Gbps | Per tunnel (v1) |
| Recovery time | Days | No formal SLA |
| Light level minimum | -20 dBm | Target threshold |
