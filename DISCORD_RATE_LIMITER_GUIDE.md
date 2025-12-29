# Discord Rate Limiter - Implementation Guide

## ✅ What Was Fixed

Your bot was getting **429 Too Many Requests** errors from Discord. The rate limiter now follows **Discord's official best practices**:

- ✅ **Parses response headers dynamically** (not hard-coded)
- ✅ **Tracks buckets per route** using Discord's bucket system
- ✅ **Monitors invalid requests** (401, 403, 429) to avoid Cloudflare bans
- ✅ **Handles shared rate limits** properly (doesn't count against you)
- ✅ **Exempts interaction endpoints** from global rate limit

## 📦 Files Modified

### 1. **Created: `utils/discord_rate_limiter.py`**
   - Parses Discord rate limit headers (`X-RateLimit-*`)
   - Tracks bucket limits dynamically
   - Monitors global rate limit (50 req/s)
   - Tracks invalid requests to avoid Cloudflare ban (10k/10min)
   - Handles 401, 403, 429 responses properly

### 2. **Updated: `menus.py`**
   - Uses dynamic rate limiting with header parsing
   - Tracks 401, 403, 429 errors as invalid requests
   - Interaction endpoints marked as exempt from global limit
   - Automatic retry with proper `Retry-After` parsing

### 3. **Updated: `bot.py`**
   - Added rate limiting to `send_discord_message()`
   - Prevents rapid channel message sends

### 4. **Updated: `handlers/handle_welcome_message.py`**
   - Added rate limiting to DM sends

## 🚀 Discord's Rate Limiting System

### Rate Limit Headers

Discord returns these headers with every response:

```http
X-RateLimit-Limit: 5           # Max requests allowed
X-RateLimit-Remaining: 3       # Requests remaining
X-RateLimit-Reset: 1640000000  # Unix timestamp when resets
X-RateLimit-Reset-After: 2.5   # Seconds until reset
X-RateLimit-Bucket: abcd1234   # Unique bucket identifier
X-RateLimit-Scope: user        # 'user', 'global', or 'shared'
```

Our rate limiter **parses these headers** and tracks limits per bucket.

### Global Rate Limit

- **50 requests per second** across all API endpoints
- Applied per bot (not per IP if authenticated)
- **Interaction endpoints are EXEMPT** ✨
- Our limiter enforces this automatically

### Invalid Request Limit (Cloudflare Ban)

Discord bans IPs that make **10,000 invalid requests in 10 minutes**:
- **401**: Unauthorized (bad token)
- **403**: Forbidden (missing permissions)
- **429**: Too Many Requests (rate limited)

**Note**: 429 with `X-RateLimit-Scope: shared` doesn't count against you!

Our limiter:
- ✅ Tracks all invalid requests
- ✅ Warns at 50% threshold (5,000 requests)
- ✅ Critical alert at 80% threshold (8,000 requests)

## 📊 Monitoring

### Check Rate Limit Statistics:

```python
from utils.discord_rate_limiter import get_rate_limit_stats, get_invalid_request_breakdown

# Overall stats
stats = get_rate_limit_stats()
print(stats)
# Output:
# {
#   'global_requests_last_second': 12,
#   'invalid_requests_last_10min': 45,
#   'tracked_buckets': 3,
#   'bucket_info': {
#     'abcd1234': {'remaining': 3, 'limit': 5, 'resets_in': 1.2}
#   }
# }

# Invalid request breakdown
breakdown = get_invalid_request_breakdown()
print(breakdown)
# Output: {429: 40, 403: 5}
```

### Log Messages to Watch For:

✅ **Normal Operation:**
```
[INFO] Received shared rate limit (429), waiting before retry
```

⚠️ **Warning (50% threshold):**
```
[WARNING] ⚠️ High invalid request rate: 5234/10000 in 10 min
```

🚨 **Critical (80% threshold):**
```
[ERROR] ⚠️ CRITICAL: 8456/10000 invalid requests in 10 min window!
[ERROR] Approaching Cloudflare ban threshold. Review error handling!
```

## 🔧 Usage in New Code

### For Interactions (Auto Rate-Limited):
```python
from utils.discord_rate_limiter import safe_followup_send, safe_response_send

# These automatically:
# - Skip global rate limit (interactions are exempt)
# - Parse response headers
# - Track buckets dynamically
# - Handle 429s with retry
await safe_followup_send(interaction, "Your message", ephemeral=True)
await safe_response_send(interaction, "Your message", ephemeral=True)
```

### For Non-Interaction Endpoints:
```python
from utils.discord_rate_limiter import wait_for_rate_limit

# Subject to global 50 req/s limit
await wait_for_rate_limit("channel_message", is_interaction=False)
await channel.send("Your message")

# For DMs
await wait_for_rate_limit("dm", is_interaction=False)
await user.send("Your DM")
```

## 🎯 Best Practices from Discord

### 1. Parse Headers, Don't Hard-Code
✅ **We do this**: Track buckets dynamically from `X-RateLimit-*` headers  
❌ **Don't do**: Hard-code delays like "wait 100ms between calls"

### 2. Handle Invalid Requests Properly

**401 (Unauthorized)**:
- Check your bot token is valid
- Stop making requests if token is invalid
- Our limiter tracks these automatically

**403 (Forbidden)**:
- Check permissions before making requests
- Don't retry forbidden operations
- Our limiter logs and tracks these

**429 (Rate Limited)**:
- Check if `X-RateLimit-Scope: shared` (doesn't count)
- Use `Retry-After` header for wait time
- Our limiter handles this automatically

### 3. Interaction Endpoints are Exempt

Discord's interaction endpoints are **NOT** subject to the global 50 req/s limit:
- `interaction.response.send_message()`
- `interaction.followup.send()`
- `interaction.edit_original_response()`

Our limiter marks these with `is_interaction=True` to skip global limit.

## ⚠️ What Caused Your Original Crash

1. **Multiple rapid API calls** without tracking buckets
2. **No header parsing** - couldn't detect exhausted buckets
3. **No invalid request tracking** - hit Cloudflare ban threshold
4. **Hard-coded delays** instead of dynamic bucket tracking

## ✨ What's Better Now

| Before | After |
|--------|-------|
| Hard-coded 100ms delays | Parses `X-RateLimit-*` headers dynamically |
| No bucket tracking | Tracks limits per Discord bucket |
| No invalid request monitoring | Warns before Cloudflare ban |
| Applied global limit to interactions | Interactions properly exempted |
| Single retry on 429 | Proper `Retry-After` parsing |
| No 401/403 handling | Tracks all invalid requests |

## 🔍 Troubleshooting

### If You Get Warnings About Invalid Requests:

**50% threshold warning**:
- Review logs for 401/403 errors
- Check permissions before making requests
- Ensure bot token is valid

**80% critical alert**:
- **Stop the bot immediately**
- Review what's causing invalid requests
- Fix permission checks or token issues
- Wait for 10-minute window to expire

### If You Still Get 429 Errors:

This is normal! The limiter:
1. Detects the 429
2. Checks if it's `shared` scope
3. Reads `Retry-After` header
4. Waits and retries automatically

You should see in logs:
```
[WARNING] Bucket abcd1234 exhausted, waiting 1.5s for reset
```

This means it's working correctly!

## 📝 Key Differences from Old Implementation

### Old (Wrong):
```python
MIN_DELAY_BETWEEN_CALLS = 0.1  # Hard-coded!
await asyncio.sleep(0.1)        # No header parsing
```

### New (Correct):
```python
# Parse headers from Discord response
bucket_info = parse_rate_limit_headers(response.headers)
update_bucket_info(bucket_info)

# Wait based on actual bucket status
if bucket['remaining'] <= 0:
    wait_time = bucket['reset_at'] - now
    await asyncio.sleep(wait_time)
```

## 🎯 Next Steps

1. **Restart your bot** to load the new rate limiter
2. **Monitor logs** for:
   - Bucket tracking messages
   - Invalid request warnings
   - Rate limit statistics
3. **Check stats periodically**:
   ```python
   from utils.discord_rate_limiter import get_rate_limit_stats
   print(get_rate_limit_stats())
   ```
4. **If you see high invalid requests**, investigate:
   - Are you checking permissions before API calls?
   - Is your bot token valid?
   - Are you handling 404s properly (e.g., deleted webhooks)?

---

**Status**: ✅ Fully compliant with Discord's official rate limiting guidelines!

**Reference**: https://discord.com/developers/docs/topics/rate-limits
