# Queues Gotchas & Troubleshooting

## Common Errors

### "Duplicate Message Processing"

**Cause:** At-least-once delivery guarantee means duplicates are possible  
**Solution:** Design consumers to be idempotent by tracking processed message IDs in KV with expiration TTL

### "Pull Consumer Can't Decode Messages"

**Cause:** Messages sent with `v8` content type are not decodable by pull consumers or dashboard  
**Solution:** Use `json` content type for pull consumers: `env.MY_QUEUE.send(data, { contentType: 'json' })`

### "Messages Not Being Delivered"

**Cause:** Queue paused, consumer not configured, or consumer errors  
**Solution:** Check queue status with `wrangler queues list`, verify consumer configured with `wrangler queues consumer add`, and check logs with `wrangler tail`

### "High Dead Letter Queue Rate"

**Cause:** Consumer repeatedly failing to process messages  
**Solution:** Review consumer error logs, check external dependency availability, verify message format matches expectations, or increase retry delay

### "CPU Time Exceeded in Consumer"

**Cause:** Consumer processing exceeding 30s default CPU time limit  
**Solution:** Increase CPU limit in wrangler.jsonc: `{ "limits": { "cpu_ms": 300000 } }` (5 minutes max)

## Limits

| Limit | Value | Notes |
|-------|-------|-------|
| Max queues | 10,000 | Per account |
| Message size | 128 KB | Maximum per message |
| Batch size (consumer) | 100 messages | Maximum messages per batch |
| Batch size (sendBatch) | 100 msgs or 256 KB | Whichever limit reached first |
| Throughput | 5,000 msgs/sec | Per queue |
| Retention | 4-14 days | Configurable retention period |
| Max backlog | 25 GB | Maximum queue backlog size |
| Max delay | 12 hours (43,200s) | Maximum message delay |
| Max retries | 100 | Maximum retry attempts |
| CPU time default | 30s | Per consumer invocation |
| CPU time max | 300s (5 min) | Configurable via `limits.cpu_ms` |
| Operations per message | 3 (write + read + delete) | Base cost per message |
| Pricing | $0.40 per 1M operations | After 1M free operations |
| Message charging | Per 64 KB chunk | Messages charged in 64 KB increments |
