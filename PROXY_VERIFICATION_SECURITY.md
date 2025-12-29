# Proxy Verification Security Guide

## Overview
The proxy verification system implements multiple layers of security to protect sensitive user data and prevent unauthorized access.

## Security Features

### 1. Token File Protection
- **Encryption**: Verification tokens are stored in encrypted files (`verification_tokens.enc`)
- **Key Management**: Uses Fernet encryption with automatically generated keys
- **File Permissions**: Attempts to set restrictive permissions (owner-only read/write)
- **Secure Wipe**: Emergency cleanup function securely wipes sensitive files

### 2. Log Security
- **IP Address Hashing**: IP addresses are SHA256 hashed before logging
- **Data Sanitization**: Sensitive data is truncated or masked in logs
- **Retention Policy**: Automatic cleanup of logs older than 30 days
- **Audit Logging**: Structured logging for security events

### 3. API Key Security
- **Encryption**: API keys are encrypted in the database using AES-256
- **Rotation**: Functions available for secure key rotation
- **Validation**: Automatic validation of API key validity
- **Access Control**: Keys are decrypted only when needed

### 4. Network Security
- **Rate Limiting**: 10 requests/minute per IP address
- **Request Validation**: Comprehensive input validation
- **CORS Protection**: Restricted to same-origin only
- **Security Headers**: XSS protection, content-type validation, etc.

## Security Audit

Run a security audit to check the current security status:

```python
from proxy_verification_server import audit_security_status

audit = audit_security_status()
print("Security Audit Results:")
for key, value in audit.items():
    print(f"{key}: {value}")
```

## Key Rotation

To rotate API keys securely:

```python
from proxy_verification_server import rotate_api_key, validate_api_key_current

# Validate current key
validation = validate_api_key_current(guild_id)
if validation['valid']:
    # Rotate to new key
    success = rotate_api_key(guild_id, "new-api-key-here")
    if success:
        print("API key rotated successfully")
```

## Log Management

### Automatic Cleanup
The system automatically cleans up old logs daily. To manually trigger cleanup:

```python
from proxy_verification_server import cleanup_old_logs

cleanup_old_logs(retention_days=30)  # Keep 30 days of logs
```

### Log Format
Verification attempts are logged in this format:
```
VERIFICATION_ATTEMPT token=abc123... ip_hash=1a2b3c4d ua=Mozilla/5.0... status=passed
```

## Emergency Procedures

### Secure Data Wipe
In case of security breach, use emergency cleanup:

```python
from proxy_verification_server import emergency_cleanup

# WARNING: This will break the verification system
emergency_cleanup()
```

## File Locations

### Sensitive Files (Gitignored)
- `verification_tokens.enc` - Encrypted token storage
- `token_key.key` - Encryption key for tokens
- `key.key` - Database encryption key

### Log Files
- `logs/*.log` - Application logs with hashed IP addresses

## Security Best Practices

1. **Regular Audits**: Run security audits periodically
2. **Key Rotation**: Rotate API keys every 90 days
3. **Log Monitoring**: Monitor verification logs for anomalies
4. **Access Control**: Limit server access to authorized personnel
5. **Backup Security**: Ensure backups of sensitive files are encrypted

## Compliance

The system is designed to comply with:
- **Data Protection**: IP addresses are hashed, sensitive data encrypted
- **Privacy Regulations**: Minimal data retention, automatic cleanup
- **Security Standards**: Encryption, access controls, audit logging

## Monitoring

Monitor these security events:
- Failed verification attempts
- Rate limit violations
- API key validation failures
- File permission changes
- Encryption key access

## Incident Response

1. **Detection**: Monitor logs for security events
2. **Containment**: Disable affected services
3. **Investigation**: Review audit logs
4. **Recovery**: Rotate keys, clean up data
5. **Lessons Learned**: Update security measures