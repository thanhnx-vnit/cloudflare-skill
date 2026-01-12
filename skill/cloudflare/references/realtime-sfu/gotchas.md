# Gotchas & Troubleshooting

## Common Errors

### "Slow initial connect (~1.8s)"

**Cause:** First STUN delayed during consensus forming (normal behavior)
**Solution:** Subsequent connections are faster. CF detects DTLS ClientHello early to compensate.

### "No media flow"

**Cause:** SDP exchange incomplete, connection not established, tracks not added before offer, browser permissions missing
**Solution:** 
1. Verify SDP exchange complete
2. Check `pc.connectionState === 'connected'`
3. Ensure tracks added before creating offer
4. Confirm browser permissions granted
5. Use `chrome://webrtc-internals` for debugging

### "Track not receiving"

**Cause:** Track not published, track ID not shared, session IDs mismatch, `pc.ontrack` not set, renegotiation needed
**Solution:** 
1. Verify track published successfully
2. Confirm track ID shared between peers
3. Check session IDs match
4. Set `pc.ontrack` handler before answer
5. Trigger renegotiation if needed

## Limits

| Resource/Limit | Value | Notes |
|----------------|-------|-------|
| Egress (Free) | 1TB/month | Per account |
| Egress (Paid) | $0.05/GB | After free tier |
| Inbound traffic | Free | All plans |
| TURN service | Free | Included with SFU |
| Participants | No hard limit | Client bandwidth/CPU bound |
| Tracks per session | No hard limit | Client resources limited |
| WebRTC ports | UDP 1024-65535 | Required for media |
