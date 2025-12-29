# Production Deployment Guide for Proxy Verification Server

## Overview

The proxy verification server has been configured for production deployment using Gunicorn as the WSGI server instead of Flask's development server.

## Production Setup

### 1. Install Production Dependencies

```bash
pip install gunicorn>=21.2.0
```

### 2. Production Server Configuration

The server automatically detects and uses Gunicorn when available:

- **Gunicorn (Production)**: Multi-worker, threaded, production-ready
- **Flask Development (Fallback)**: Single-threaded, development only

### 3. Gunicorn Configuration

The bot automatically starts Gunicorn with these production settings:

```bash
gunicorn \
  --bind 0.0.0.0:5000 \
  --workers 1 \
  --threads 2 \
  --worker-class gthread \
  --access-logfile - \
  --error-logfile - \
  --log-level warning \
  --timeout 30 \
  --keep-alive 10 \
  proxy_verification_web:app
```

### 4. Standalone Production Server

For dedicated deployment, use the startup script:

```bash
# Automatic server selection (Gunicorn preferred)
python start_proxy_server.py

# Or manually with Gunicorn
gunicorn -w 2 -b 0.0.0.0:5000 proxy_verification_web:app

# Or manually with Flask (not recommended)
python proxy_verification_web.py
```

## Production Features

### ✅ Production-Ready Configuration

- **Environment**: `app.config['ENV'] = 'production'`
- **Debug Mode**: Disabled
- **Testing Mode**: Disabled
- **Reloader**: Disabled (prevents file watching in production)

### ✅ WSGI Server (Gunicorn)

- **Multi-worker**: Handles multiple concurrent requests
- **Threaded workers**: Efficient for I/O-bound tasks
- **Timeout handling**: Prevents hanging requests
- **Keep-alive**: Optimizes connection reuse
- **Logging**: Structured access and error logs

### ✅ Security Headers

- **CORS**: Properly configured for cross-origin requests
- **No debug info**: Sensitive information not exposed

### ✅ Error Handling

- **Graceful shutdown**: Proper cleanup on termination
- **Exception handling**: Comprehensive error catching
- **Logging**: All errors logged appropriately

## Windows Compatibility

**Important**: Gunicorn is not compatible with Windows systems due to Unix-specific dependencies. On Windows:

- The server automatically detects Windows and uses Flask development server
- Gunicorn installation is ignored on Windows platforms
- For production deployment, use Linux/Unix systems with Gunicorn
- The Flask development server provides full functionality on Windows for development/testing

### Windows-Specific Behavior

```bash
# On Windows, this will always use Flask:
python start_proxy_server.py
# Output: "Gunicorn not found - using Flask development server"
# Even if Gunicorn is installed, Windows detection overrides it
```

## Deployment Options

### Option 1: Integrated with Bot (Recommended)

The bot automatically starts the proxy verification server when it starts up. No additional configuration needed.

### Option 2: Standalone Server

Run the proxy verification server separately:

```bash
# Using the startup script (auto-detects Gunicorn)
python start_proxy_server.py

# Or using systemd service
sudo nano /etc/systemd/system/proxy-verification.service
```

Example systemd service file:

```ini
[Unit]
Description=Proxy Verification Web Server
After=network.target

[Service]
Type=simple
User=your-user
WorkingDirectory=/path/to/ns_echo_bot
ExecStart=/path/to/python start_proxy_server.py
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Then enable and start:

```bash
sudo systemctl enable proxy-verification
sudo systemctl start proxy-verification
sudo systemctl status proxy-verification
```

### Option 3: Docker Deployment

Create a Dockerfile:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["gunicorn", "-w", "2", "-b", "0.0.0.0:5000", "proxy_verification_web:app"]
```

Build and run:

```bash
docker build -t proxy-verification .
docker run -p 5000:5000 proxy-verification
```

## Monitoring and Maintenance

### Health Check

```bash
curl http://localhost:5000/health
```

Expected response:
```json
{
  "status": "healthy",
  "timestamp": "2025-12-03T...",
  "service": "proxy-verification-server"
}
```

### Logs

- **Gunicorn**: Access and error logs go to stdout/stderr
- **Application**: Uses the bot's logging system
- **Systemd**: `journalctl -u proxy-verification`

### Performance Tuning

Adjust Gunicorn settings based on load:

```bash
# Low traffic
gunicorn -w 1 -b 0.0.0.0:5000 proxy_verification_web:app

# Medium traffic
gunicorn -w 2 -b 0.0.0.0:5000 proxy_verification_web:app

# High traffic
gunicorn -w 4 -b 0.0.0.0:5000 proxy_verification_web:app
```

## Troubleshooting

### Common Issues

1. **Port 5000 already in use**
   ```bash
   sudo lsof -i :5000
   sudo kill -9 <PID>
   ```

2. **Gunicorn not found**
   ```bash
   pip install gunicorn
   ```

3. **Permission denied**
   ```bash
   sudo chown your-user:your-user /path/to/ns_echo_bot
   ```

4. **Connection refused**
   - Check if server is running: `ps aux | grep gunicorn`
   - Check firewall: `sudo ufw status`
   - Check port binding: `netstat -tlnp | grep 5000`

### Debug Mode (Temporary Only)

For debugging only, you can enable Flask debug mode:

```python
app.run(debug=True)  # NEVER use in production
```

But remember: **Flask debug mode exposes sensitive information and is not secure for production use.**

## Files Modified

- `proxy_verification_web.py`: Production configuration
- `start_proxy_server.py`: Gunicorn integration
- `bot.py`: Production server startup
- `requirements.txt`: Added Gunicorn dependency