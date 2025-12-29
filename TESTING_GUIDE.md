# 🧪 Testing Guide: Citizenship Application Proxy Check System

## Pre-Deployment Testing Checklist

### Phase 1: Setup & Configuration

- [ ] **Database Migration**
  - Start bot: `python bot.py`
  - Check console for: `✅ citizenship_applications migration complete`
  - Verify in database: 4 new columns exist
    ```sql
    PRAGMA table_info(citizenship_applications);
    ```

- [ ] **Verification Server**
  - Start server: `python simple_server.py`
  - Check console for: `Server running on http://localhost:5000`
  - Access health check: http://localhost:5000/health
  - Should return: `{"status": "healthy"}`

- [ ] **API Key Configuration**
  - Get key from https://proxycheck.io (free account)
  - In Discord: `?menu` → Setup & Settings → Setup Proxy Check API Key
  - Paste key when prompted
  - System should encrypt and store

- [ ] **Syntax Validation**
  - Run: `python -m py_compile proxy_verification_server.py`
  - Run: `python -m py_compile db_echo.py`
  - No errors should appear

---

### Phase 2: IP Capture & Encryption

- [ ] **Create Test Application**
  - User: `?citizenship`
  - Answer: Q1: "Test Nation" | Q2: "Testing" | Q3: "Test"
  - Receive verification URL
  - Check console for: `Created verification link for application X`

- [ ] **Verify Token Generation**
  - URL format: `http://localhost:5000/verify/auto/{TOKEN}`
  - TOKEN should be: 43+ characters, URL-safe
  - Check `verification_tokens.enc` exists
  - File should be encrypted (non-readable)

- [ ] **Test IP Capture**
  - Click verification URL
  - Browser shows loading page
  - Check server console for: `Auto-verification for token: ...`
  - Should see IP address logged

- [ ] **Verify IP Encryption**
  - Check database: `SELECT ip_addresses FROM citizenship_applications`
  - Should NOT see readable IP (should be encrypted)
  - Example: `gAAAAABl...{encrypted}...==`

- [ ] **Verify IP Hashing**
  - Check database: `SELECT ip_hash, ip_prefix_hash FROM citizenship_applications`
  - Both should contain: SHA-256 hex strings (64 chars)
  - Should NOT match IP address directly

---

### Phase 3: Proxy Check (Existing Feature - Verify Still Works)

- [ ] **Test Proxy Check Pass**
  - User IP: Non-VPN (residential IP)
  - Click verification URL
  - Check result: `proxy_verification_status = 'passed'`
  - Check database: `proxy_verification_status = 'passed'`

- [ ] **Test Proxy Check Fail**
  - Use VPN to click URL
  - Check result: Should show "proxy/VPN detected"
  - Check database: `proxy_verification_status = 'failed'`

- [ ] **Test API Key Validation**
  - Try with wrong API key: Should fail with error
  - Try without API key: Should show "not configured"
  - Try with valid key: Should succeed

---

### Phase 4: Resident IP Matching (NEW FEATURE)

#### Setup:
```sql
-- Add test residents with known IPs (use your own IP)
INSERT INTO nations (name) VALUES ('Test Resident 1');
INSERT INTO residents (nation_id, ip_addresses, banned) 
VALUES (1, (encrypted_ip), 0);

INSERT INTO nations (name) VALUES ('Test Resident 2');
INSERT INTO residents (nation_id, ip_addresses, banned) 
VALUES (2, (encrypted_ip), 1);  -- This one is banned
```

- [ ] **Test Resident Match Detection**
  - Use same IP as test residents
  - Click verification URL
  - Check database:
    - `resident_match_count = 2`
    - `resident_matches` contains JSON with 2 entries
  - Should show warning in browser

- [ ] **Test Banned Resident Detection**
  - Check database:
    - `banned_match_count = 1`
    - `banned_matches` contains JSON with banned resident
  - Browser should show: `🚫 ALERT: IP matches 1 BANNED resident(s)!`

- [ ] **Test JSON Format**
  - Check `resident_matches` column:
    ```json
    [
      {
        "nation_name": "Test Resident 1",
        "nation_id": 1,
        "banned": false,
        "region": "Test Region",
        "match_type": "exact"
      },
      {
        "nation_name": "Test Resident 2",
        "nation_id": 2,
        "banned": true,
        "region": "Test Region",
        "match_type": "exact"
      }
    ]
    ```

- [ ] **Test No Matches**
  - Use different IP than residents
  - Click verification URL
  - Check database:
    - `resident_match_count = 0` (or NULL)
    - `resident_matches = NULL`
    - `banned_match_count = 0` (or NULL)
    - `banned_matches = NULL`

---

### Phase 5: Database Storage

- [ ] **Full Record Check**
  ```sql
  SELECT 
    id,
    nation_name,
    ip_addresses,
    ip_hash,
    ip_prefix_hash,
    proxy_verification_status,
    resident_match_count,
    resident_matches,
    banned_match_count,
    banned_matches,
    status
  FROM citizenship_applications
  WHERE id = {test_app_id};
  ```
  - All columns should be populated
  - No NULL values except where expected
  - IP should be encrypted (not readable)

- [ ] **JSON Validation**
  - Parse `resident_matches` as JSON
  - Parse `banned_matches` as JSON
  - Both should be valid JSON or NULL
  - No truncation or corruption

---

### Phase 6: Discord Integration

- [ ] **Posting to Thread**
  - Check that application posts to transcript channel
  - Thread should show:
    - ✅ Proxy status
    - ⚠️ Resident matches count
    - 🚫 Banned matches (if any)
    - Application details

- [ ] **Admin Review Menu**
  - `?menu` → Citizenship Applications → Review Applications
  - Should show: Nation, Status, Matches, Banned Matches
  - Click to view full details

- [ ] **Approve/Reject Buttons**
  - Approve button should add to residents
  - Reject button should set status to "rejected"
  - Notes should be saved

---

### Phase 7: Error Handling

- [ ] **Test Invalid Token**
  - Try URL with fake token
  - Should show: "Invalid or expired verification token"
  - Database should not be updated

- [ ] **Test Expired Token**
  - Create application
  - Wait > 5 minutes
  - Try verification URL
  - Should show: "Verification token has expired"

- [ ] **Test Duplicate Verification**
  - Complete verification once
  - Try again with same token
  - Should show: "Verification already completed"

- [ ] **Test Rate Limiting**
  - Send 11 requests within 1 minute from same IP
  - 11th should return: 429 (Too Many Requests)
  - Should not cause errors in database

- [ ] **Test Network Errors**
  - Stop proxycheck.io (simulate API down)
  - Try verification
  - Should show graceful error message
  - Should not corrupt database

---

### Phase 8: Security Verification

- [ ] **IP Not in Logs**
  - Check bot console logs
  - Should NOT see actual IP addresses in plain text
  - Should only see hashes or masked IPs

- [ ] **API Key Encryption**
  - Check database: `SELECT proxy_check_api_key FROM setup`
  - Should NOT see readable API key
  - Should see encrypted value starting with `gAAAAAA...`

- [ ] **Token File Encryption**
  - Open `verification_tokens.enc` in text editor
  - Should NOT see readable content
  - Should be binary/encrypted data

- [ ] **CORS Headers**
  - Use browser DevTools to check response headers
  - Should see: `X-Content-Type-Options: nosniff`
  - Should see: `X-Frame-Options: DENY`
  - Should see: `X-XSS-Protection: 1; mode=block`

- [ ] **Input Validation**
  - Try SQL injection in form fields
  - Try XSS payload in form fields
  - Should be properly escaped/rejected

---

### Phase 9: Multiple Users

- [ ] **Concurrent Verification**
  - Have 2-3 users verify simultaneously
  - All should complete without errors
  - Each should have correct data in database
  - No data corruption or conflicts

- [ ] **Different Guilds**
  - Create test apps in different guilds
  - Each should post to correct channel
  - No cross-guild contamination

---

### Phase 10: Edge Cases

- [ ] **Very Long Nation Names**
  - Try 100+ character nation name
  - Should store and display correctly

- [ ] **Special Characters**
  - Try nation name with emojis, symbols
  - Should handle gracefully

- [ ] **IPv6 Addresses**
  - Test with IPv6 IP (if available)
  - Should hash and store correctly

- [ ] **No Residents in Database**
  - With empty residents table
  - Should return: matches=0, no errors

- [ ] **Corrupt JSON in Database**
  - Manually set invalid JSON in resident_matches
  - Admin should still see application
  - Should not crash menu system

---

## 📋 Test Report Template

Use this to document your testing:

```
DATE: 2025-12-03
TESTER: [Your Name]
VERSION: [Bot Version]

PHASE 1: Setup & Configuration
- [ ] Database migration: PASS/FAIL
- [ ] Server startup: PASS/FAIL
- [ ] API key config: PASS/FAIL
- [ ] Syntax validation: PASS/FAIL

PHASE 2: IP Capture & Encryption
- [ ] Token generation: PASS/FAIL
- [ ] IP capture: PASS/FAIL
- [ ] IP encryption: PASS/FAIL
- [ ] IP hashing: PASS/FAIL

PHASE 3: Proxy Check
- [ ] Proxy pass: PASS/FAIL
- [ ] Proxy fail: PASS/FAIL
- [ ] API key validation: PASS/FAIL

PHASE 4: Resident Matching
- [ ] Match detection: PASS/FAIL
- [ ] Banned detection: PASS/FAIL
- [ ] JSON format: PASS/FAIL
- [ ] No matches case: PASS/FAIL

PHASE 5: Database Storage
- [ ] Full record check: PASS/FAIL
- [ ] JSON validation: PASS/FAIL

PHASE 6: Discord Integration
- [ ] Thread posting: PASS/FAIL
- [ ] Review menu: PASS/FAIL
- [ ] Approve/Reject: PASS/FAIL

PHASE 7: Error Handling
- [ ] Invalid token: PASS/FAIL
- [ ] Expired token: PASS/FAIL
- [ ] Duplicate verification: PASS/FAIL
- [ ] Rate limiting: PASS/FAIL
- [ ] Network errors: PASS/FAIL

PHASE 8: Security
- [ ] IP not in logs: PASS/FAIL
- [ ] API key encryption: PASS/FAIL
- [ ] Token encryption: PASS/FAIL
- [ ] CORS headers: PASS/FAIL
- [ ] Input validation: PASS/FAIL

PHASE 9: Multiple Users
- [ ] Concurrent verification: PASS/FAIL
- [ ] Different guilds: PASS/FAIL

PHASE 10: Edge Cases
- [ ] Long names: PASS/FAIL
- [ ] Special chars: PASS/FAIL
- [ ] IPv6: PASS/FAIL
- [ ] Empty residents: PASS/FAIL
- [ ] Corrupt JSON: PASS/FAIL

OVERALL: PASS / FAIL

ISSUES FOUND:
1. [Issue description]
2. [Issue description]

NOTES:
[Any additional notes]
```

---

## 🚀 When Everything Passes

Once all tests pass:

1. ✅ Code is production-ready
2. ✅ All features working
3. ✅ Security verified
4. ✅ Error handling complete
5. ✅ Documentation complete

**You're ready to deploy!**

---

## 📞 Troubleshooting Common Issues

### "IP not captured"
- Check server is running
- Check request actually comes from user browser
- Check URL is correct

### "Resident matching returns 0"
- Verify residents table has entries
- Verify resident IPs are encrypted (not readable)
- Verify user IP format is correct

### "Banned detection not working"
- Check banned column = 1 for test resident
- Run migration: `init_echo_db()`
- Check decrypt function working

### "Discord not posting"
- Check bot has permissions in channel
- Check thread creation enabled
- Check JSON is valid (parse manually)

### "Token expiration not working"
- Verify token file is being saved
- Check system time is correct
- Verify 5-minute timeout in code

---

**Happy testing! 🎉**
