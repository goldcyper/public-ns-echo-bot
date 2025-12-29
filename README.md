# 🎯 Echo Bot - NationStates Discord Bot

**A powerful Discord bot for managing NationStates regions with automated World Assembly monitoring, telegram campaigns, and secure citizenship applications.**

![Python](https://img.shields.io/badge/Python-3.11+-blue) ![Discord.py](https://img.shields.io/badge/Discord.py-2.3+-blue) ![License](https://img.shields.io/badge/License-MIT-green)

---

## What Does This Bot Do?

Echo Bot helps you manage your NationStates region directly from Discord:

- 📊 **Track World Assembly Activity** - Automatically monitor proposals and voting
- 📨 **Send Automated Telegrams** - Welcome new nations, recruit, and manage endorsements  
- 👥 **Verify Nation Ownership** - Securely link Discord users to their nations
- 🏛️ **Process Applications** - Built-in citizenship application system with proxy checking
- 🔍 **Search Residents** - Find and manage resident data across servers
- 🌐 **Multi-Server Ready** - Run the same bot on multiple Discord servers with different settings

---

## Quick Start (3 Steps)

### 1. Install

```powershell
git clone https://github.com/yourusername/ns_echo_bot.git
cd ns_echo_bot
pip install -r requirements.txt
```

### 2. Start the Bot

```powershell
python bot.py
```

The bot will prompt you for your Discord bot token on first run.

### 3. Configure in Discord

In your Discord server, type:
```
?menu
```

Then follow the interactive setup:
1. **Setup Bot Permissions** - Add yourself as admin
2. **Setup X-Auto-Login** - Connect your NationStates account
3. Done! The bot is ready to use.

---

## Main Features Explained

### 🏛️ World Assembly Monitoring

The bot watches the World Assembly and can:
- Post new proposals to a Discord channel
- Track voting status
- Notify about new WA members in your region
- Post resolution results to your region's message board

**Setup:** Use `?menu` → Channel Setup → Setup WA Channel

### 📨 Telegram Automation

Send automatic telegrams for four purposes:
- **Recruitment** - Welcome new nations to your region
- **Restorer** - Target nations that recently moved regions
- **Endorsements** - Request endorsements from WA members
- **Over Cap** - Ask nations to reduce endorsements if over your limit

**Setup:** Use `?menu` → Telegram Automation → configure each type

### 👥 Nation Verification

Let Discord users prove they own a NationStates nation:

```
?verify Nation_Name
```

The bot sends a DM with instructions to verify via NationStates.net. Once verified, you can use commands like:

```
?whois @username          # See which nation this Discord user owns
?whois Nation_Name        # See which Discord user owns this nation
```

### 🏛️ Citizenship Applications

Users can apply for citizenship with:

```
?citizenship
```

The bot will:
1. Ask questions via DM (nation name, reason for joining, etc.)
2. Send a verification link to check for proxies/VPNs
3. Post the complete application to your transcript channel for review

**Setup:** Use `?menu` → Citizenship Applications → Setup Transcript Channel

### 🔍 Resident Search

Search your resident database:

```
?listresident john        # Search for "john" in your server
?residents john          # Search across all authorized servers
```

Find residents by nation name, Discord username, or other details.

---

## Security Features

Your data is protected with:

- 🔐 **Encryption** - All passwords and API keys encrypted with AES-256
- 🛡️ **Proxy Detection** - Built-in proxy/VPN checking for applications
- 🔒 **Local Server** - Runs on your machine, no external hosting needed
- ⏱️ **Rate Limiting** - Prevents abuse of verification system
- 📝 **Audit Logs** - Track all security events

---

## Requirements

- **Python 3.11 or newer** 
- **Discord Bot Token** - Get one from [Discord Developer Portal](https://discord.com/developers/applications)
- **NationStates Account** - You'll need your nation's email and password

---

## Common Commands

Once set up, here are the commands you'll use most:

| Command | What It Does |
|---------|--------------|
| `?menu` | Opens the interactive configuration menu |
| `?guild_config` | Shows your server's current settings |
| `?verify Nation_Name` | Verify you own a nation |
| `?whois @user` | See which nation a Discord user owns |
| `?newnations Lazarus` | List new WA nations in a region |
| `?listresident john` | Search for residents |
| `?citizenship` | Start a citizenship application |
| `?telegram_status` | Check telegram automation status |
| `?health` | Check bot status |

---

## Multi-Server Support

You can run this bot on multiple Discord servers at once! Each server can:
- Monitor different NationStates regions
- Use different credentials
- Have independent settings

**Example:** Server A manages "Lazarus", Server B manages "The Pacific" - both using the same bot.

See [MULTI_SERVER_SETUP.md](MULTI_SERVER_SETUP.md) for details.

---

## Troubleshooting

**Bot not starting?**
- Make sure Python 3.11+ is installed: `python --version`
- Install dependencies: `pip install -r requirements.txt`
- Check if Discord bot token is correct

**Bot not responding to commands?**
- Make sure the bot is online in Discord
- Check that bot has permissions to read/send messages
- Verify you're using the correct command prefix (default is `?`)

**No credentials configured?**
- Use `?menu` → Setup & Settings → Setup X-Auto-Login
- Bot will DM you for your NationStates login details

**Verification not working?**
- Make sure you're copying the code from NationStates within 2 minutes
- Check nation name is spelled correctly (use underscores, not spaces)

**Proxy verification failing?**
- Check that proxycheck.io API key is configured: `?menu` → Setup Proxy Check API Key
- Make sure local server is running (starts automatically with bot)

**Need more help?**
- Check the [TESTING_GUIDE.md](TESTING_GUIDE.md) for detailed troubleshooting
- Open an issue on [GitHub Issues](https://github.com/yourusername/ns_echo_bot/issues)

---

## Advanced Features

### Cross-Server Resident Sharing

Let multiple Discord servers access the same resident database:

1. Server A admin grants access to Server B via `?menu` → Residents Management
2. Users in Server B can now search Server A's residents with `?residents`
3. IP addresses only shown to admins for privacy

Perfect for regional organizations running multiple Discord servers.

### Optional Echo API

For advanced users who want historical data tracking, you can run the Echo API service:

```powershell
python echo_api.py
```

**What is it?**
- Background service that downloads daily NationStates data dumps
- Processes nation and region data for historical tracking
- Stores data in `echo_api.db`

**Do I need it?**
- **No** - The bot works fine without running echo_api.py
- On first startup, the bot auto-imports data from backup files in `echo_api_deploy/`
- This gives you immediate access to nation/region data without waiting for downloads

**When to run it:**
- If you want daily automatic data updates
- If you need the absolute latest nation/region information
- If you're tracking historical nation movements

Most users can skip running echo_api.py and rely on the pre-imported data.

---

## Documentation

For more detailed information:

| Guide | What's Inside |
|-------|---------------|
| [Multi-Server Setup](MULTI_SERVER_SETUP.md) | Running the bot on multiple Discord servers |
| [Telegram Targeting Guide](TELEGRAM_TARGETING_GUIDE.md) | Configuring automated telegram campaigns |
| [Residents Import Guide](RESIDENTS_IMPORT_GUIDE.md) | Importing resident data from CSV/Google Sheets |
| [Memory Monitor Guide](MEMORY_MONITOR_GUIDE.md) | Monitoring bot performance and memory usage |
| [Proxy Verification Security](PROXY_VERIFICATION_SECURITY.md) | Security details for citizenship applications |
| [Production Deployment](PRODUCTION_DEPLOYMENT_GUIDE.md) | Setting up the bot on a server with systemd |
| [Testing Guide](TESTING_GUIDE.md) | Running tests and troubleshooting |

---

## For Developers

### Database Management
```powershell
# Export database
python .\scripts\export_db_parts_sql.py --db .\echo_api.db

# Check Python compatibility
python .\scripts\verify_python_compat.py
```

### Running Tests
```powershell
pytest tests/
python -m py_compile bot.py
```

### Code Style
- Follow PEP 8
- Use type hints
- Add docstrings to functions
- Test your changes

---

## Contributing

Want to help improve Echo Bot? Here's how:

**Report Bugs**
1. Check if the issue already exists in [GitHub Issues](https://github.com/yourusername/ns_echo_bot/issues)
2. Create a new issue with details about the problem
3. Include error messages and steps to reproduce

**Suggest Features**
1. Open a feature request on GitHub
2. Explain what you'd like added and why it's useful

**Submit Code**
1. Fork the repository
2. Create a new branch: `git checkout -b feature/your-feature`
3. Make your changes and test them
4. Submit a pull request

---

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) for details.

---

## Support

**Need Help?**

- 📖 Check the documentation guides above
- 🐛 Report bugs on [GitHub Issues](https://github.com/yourusername/ns_echo_bot/issues)
- 💬 Ask questions in [GitHub Discussions](https://github.com/yourusername/ns_echo_bot/discussions)

---

**Made with ❤️ for the NationStates community**

*If this bot helps your region, please star the repository! ⭐*
