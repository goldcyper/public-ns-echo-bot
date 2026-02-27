<div align="center">

# 🌍 NS Echo Bot

### **The Ultimate NationStates Discord Bot for Nation Lookup & Region Monitoring**

[![Python Version](https://img.shields.io/badge/python-3.11%2B-blue.svg)](https://www.python.org/downloads/)
[![Discord.py](https://img.shields.io/badge/discord.py-2.3%2B-blue.svg)](https://github.com/Rapptz/discord.py)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-active-success.svg)]()

**[Features](#-key-features) • [Quick Start](#-quick-start) • [Commands](#-commands-reference) • [Support](#-support)**

---

### 🎯 What is NS Echo Bot?

A powerful, multi-guild Discord bot built for NationStates communities. Get detailed nation information, look up endorsement data, verify nation ownership, search residents, and explore WA resolutions—all from Discord!

### 🌟 Key Features

✨ **Multi-Server Support** - Manage multiple Discord servers with isolated configurations  
🔐 **Enterprise Security** - AES-256 encryption for all sensitive data  
📊 **Real-Time NS Data** - Live integration with NationStates API  
🔍 **Advanced Lookups** - Nation info, endorsement data, region history  
👥 **Resident Management** - Track and search residents across authorized servers  
🏛️ **WA Tracking** - Monitor WA nations, endorsements, resolutions & voting patterns  
🎓 **Secure Verification** - Nation ownership verification with IP tracking  
⚡ **High Performance** - Optimized memory usage and concurrent processing  

</div>

---

## 📖 Table of Contents

### 🚀 Getting Started
- [Quick Start](#-quick-start)
- [Installation](#-installation)
- [Requirements](#-requirements)

### ⚙️ Configuration
- [CLI Tools Reference](#-cli-tools-reference)
- [Basic Setup](#-basic-setup)
- [Multi-Server Setup](#-multi-server-setup)

### 📝 Commands & Features
- [Commands Reference](#-commands-reference)
- [World Assembly Features](#-world-assembly-features)
- [Resident Management](#-resident-management)

### 🔒 Security & Deployment
- [Security & Privacy](#-security--privacy)
- [Production Deployment](#-production-deployment)

### 👨‍💻 Advanced
- [Developer Guide](#-developer-guide)
- [Troubleshooting](#-troubleshooting)
- [Werewolf Game Module](#-werewolf-game-module-optional)

---

## 🚀 Quick Start

### ⚡ Installation

**Step 1: Clone the Repository**
```bash
git clone https://github.com/goldcyper/ns_echo_bot.git
cd ns_echo_bot
```

**Step 2: Create Virtual Environment & Install Dependencies**
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 🔑 First Run

**Using the CLI (Recommended):**
```bash
# 1. Setup Discord bot token
python cli/cli_setup.py token

# 2. Configure your guild
python cli/cli_setup.py guild

# 3. Start the bot
python bot.py
```

**Or Using Discord (Interactive Menu):**
```bash
python bot.py
```

Then in Discord:
```
?menu
```

Use the interactive menu to configure:
1. 👤 **Bot Permissions** - Add yourself as admin
2. 🔐 **Auto-Login** (Optional) - Add NationStates credentials
3. 📺 **Channels** (Optional) - Set up logging channels
4. 🌍 **Regions** (Optional) - Configure monitored regions

### 📦 Requirements

**Required:**
- 🐍 Python 3.11 or higher
- 💬 discord.py >= 2.3.0
- 🌐 requests >= 2.31.0
- 💻 psutil >= 5.9.0
- 🔐 cryptography >= 41.0.0
- 📄 lxml >= 4.9.0
- ⚡ aiohttp >= 3.8.0

---

## 🛠️ CLI Tools Reference

### 📋 Overview

The bot includes three CLI tools located in the `cli/` directory:

| Tool | Purpose |
|------|---------|
| 🔧 **cli_setup.py** | Bot configuration & credentials |
| 🚀 **cli_server.py** | Service management & monitoring |
| 💾 **cli_data.py** | Database operations & imports |

### 🔧 cli_setup.py - Bot Configuration

Configure Discord bot token and guild settings.

**Setup Bot Token:**
```bash
python cli/cli_setup.py token
```
Encrypts and stores your Discord bot token securely using AES-256.

**Setup Guild Configuration:**
```bash
python cli/cli_setup.py guild
```
Interactive menu for:
- 🌍 Add/remove guild regions
- 🔐 Configure API credentials (nation/password)
- ✅ Enable/disable regions for monitoring

### 🚀 cli_server.py - Server Management

Start, stop, and monitor bot services.

**Start Discord Bot:**
```bash
python cli/cli_server.py bot
```

**Start Echo API:**
```bash
python cli/cli_server.py api
```

**Start Both Services:**
```bash
python cli/cli_server.py both
```

**Check Status:**
```bash
python cli/cli_server.py status
```

### 💾 cli_data.py - Data Management

Manage database imports, exports, and resident data.

**Import Database:**
```bash
python cli/cli_data.py import echo_api_deployment.db.gz
```

**Create Backup:**
```bash
python cli/cli_data.py backup
```

**Import Residents from CSV:**
```bash
python cli/cli_data.py residents residents.csv
```

**Import Residents from Google Sheets:**
```bash
python cli/cli_data.py residents "https://docs.google.com/spreadsheets/d/SHEET_ID/edit"
```

---

## ⚙️ Basic Setup

### Configuration via `?menu`

In Discord, use the interactive menu to configure all settings:

```
?menu
```

**Main Options:**
- 🔧 **Setup & Settings** - Bot permissions, auto-login, prefix, proxy API key
- 📺 **Channel Setup** - Configure log channels
- 🌍 **Region Management** - Add/remove regions
- 📋 **Citizenship Applications** - Setup application system
- 👥 **Residents Management** - Import and manage residents
- 🎮 **Telegram Automation** - Configure telegram campaigns
- 📝 **WA Update Settings** - Configure proposal and resolution tracking

### Viewing Current Configuration

```
?guild_config
```

Shows your server's current configuration including:
- 🌍 Monitored regions
- 🔐 Active credentials
- 📺 Configured channels
- 👥 Resident statistics
- ✅ Verification data

---

## 🌐 Multi-Server Setup

### Overview

Run Echo Bot on multiple Discord servers simultaneously with completely isolated configurations:

```
Bot Instance
├── 🏛️ Server A (Lazarus)
│   ├── Region: Lazarus
│   ├── Credentials: your_nation_a + password_a
│   └── Settings: Independent configuration
└── 🌊 Server B (The East Pacific)
    ├── Region: The East Pacific
    ├── Credentials: your_nation_b + password_b
    └── Settings: Independent configuration
```

### Setup Steps

#### 1. Invite Bot to Multiple Servers

Use the same Discord bot token for all servers.

**Generate your bot invite link at:** [Discord Developer Portal](https://discord.com/developers/applications)

**On Server A:**
```
?menu → Setup & Settings → Add User ID for Bot Permissions
?menu → Region Management → Enable Region
```

**On Server B:**
```
?menu → Setup & Settings → Add User ID for Bot Permissions
?menu → Region Management → Enable Region
```

#### 2. Verify Isolation

Run in each server:
```
?guild_config
```

Each server shows its own independent configuration and statistics.

---

## 📝 Commands Reference

### 🔐 Verification Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `?verify <nation>` | Verify you own a NationStates nation | `?verify Lazarus` |
| `?whois <user/nation>` | Check if user/nation is verified | `?whois @User` or `?whois Lazarus` |
| `?proxy` | Check your IP security (VPN/proxy detection) | `?proxy` |

### 📊 Nation Lookup Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `?nation <nation>` | Get detailed nation info, region history | `?nation Lazarus` |
| `?endorse <nation>` | Show nations this nation hasn't endorsed yet | `?endorse Lazarus` |
| `?spdr <nation>` | Show a nation's influence score | `?spdr Lazarus` |
| `?nsregion <region>` | Get detailed region information | `?nsregion Lazarus` |

### 👥 Resident Commands

| Command | Description | Admin Only | Usage |
|---------|-------------|:----------:|--------|
| `?residents [search]` | Search residents across authorized servers | ❌ | `?residents john` |
| `?listresident [search]` | Search residents in this server | ✅ | `?listresident john` |
| `?resident <nation>` | Show detailed resident information | ❌ | `?resident Lazarus` |

### 🏛️ Endorsement Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `?ne <nation>` | List nations endorsing this WA nation | `?ne Lazarus` |
| `?nec <nation>` | Count nations endorsing this WA nation | `?nec Lazarus` |
| `?nne <nation>` | List regional WA nations NOT endorsing this nation | `?nne Lazarus` |
| `?nnec <nation>` | Count regional WA nations NOT endorsing this nation | `?nnec Lazarus` |

### 🌍 WA & Region Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `?newnations [region]` | Show recent WA applicants | `?newnations Lazarus` |
| `?regionupdate <region>` | Show region update times | `?regionupdate Lazarus` |
| `?updateallregions` | Update all region times (admin only) | `?updateallregions` |

### ⚖️ World Assembly Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `?ga [id] [options]` | Show GA resolution | `?ga` or `?ga 123` or `?ga 123 votes` |
| `?sc [id] [options]` | Show SC resolution | `?sc` or `?sc 456 text votes` |
| `?check_proposals` | Manually check for new proposals (admin) | `?check_proposals` |

**Resolution Display Options:**
- `text` - Show full resolution text
- `votes` - Show voting statistics
- `delegates` - Show top 10 delegate votes
- `nations` - Show nation vote counts

### 🎴 Trading Card Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `?card [season] <nation/id>` | Get trading card info | `?card Lazarus` or `?card 5 Lazarus` |
| `?deck <nation>` | Get nation's trading card deck | `?deck Lazarus` |

### 🎛️ Bot Management Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `?menu` | Open interactive configuration menu | `?menu` |
| `?guild_config` | Display current guild configuration | `?guild_config` |
| `?health` | Check bot health and running tasks (admin) | `?health` |
| `?memory` | Check memory usage (admin) | `?memory` |
| `?telegram_task_status` | Check telegram automation status (admin) | `?telegram_task_status` |

---

## 🌍 World Assembly Features

### 📊 Automatic Monitoring

The bot can optionally monitor:
- ✅ New WA proposals (checks automatically)
- ✅ New WA resolutions
- ✅ WA voting patterns
- ✅ Nation endorsements

### Resolution Lookup

**View Current Resolutions:**
```
?ga          # General Assembly current resolution
?sc          # Security Council current resolution
```

**View Historical Resolutions:**
```
?ga 123      # GA resolution #123
?sc 456      # SC resolution #456
```

**Display Full Details:**
```
?ga 123 text votes delegates    # Show text, voting stats, and top delegates
```

### Proposal Tracking

**Manual Check:**
```
?check_proposals    # Force immediate check for new proposals
```

The bot tracks proposals and posts them to Discord when they meet criteria.

---

## 👥 Resident Management

### Overview

Track and manage residents with:
- Bulk import from CSV/Google Sheets
- Cross-server search with privacy controls
- IP address tracking (encrypted)
- Verification status tracking

### Import Residents

**From CSV File:**

Required columns: `nation`

Optional columns: `discord_id`, `discord_username`, `ip_address`, `notes`, `banned`

```bash
python cli/cli_data.py residents residents.csv
```

**From Google Sheets:**
```bash
python cli/cli_data.py residents "https://docs.google.com/spreadsheets/d/SHEET_ID/edit"
```

### Search Residents

**Local Search (Current Server):**
```
?listresident john
```

**Cross-Server Search:**
```
?residents john
```

### Privacy Controls

**Cross-Server Access:**
- 👥 Other servers can search your resident database
- 🔐 IP addresses only visible to origin server admins
- 👀 Other servers see nation names and basic info only
- ❌ Access can be revoked anytime

---

## 🔐 Security & Privacy

### Encryption

**Encrypted Data:**
- 🔐 Discord bot token (AES-256)
- 🔐 NationStates passwords (AES-256)
- 🔐 IP addresses (AES-256)
- 🔐 API keys (AES-256)
- 🔐 Verification tokens (Fernet)

**Encryption Key:**
- Generated on first run
- Stored in `config/key.key`
- **Keep this file secure!**
- Loss of key = loss of encrypted data

### IP Address Security

**Storage:**
- IP addresses encrypted before storage
- SHA-256 hashes stored for matching
- Prefix hashes for subnet matching

**Access:**
- Only admins can view IPs
- Only admins of the origin server
- Audit logging for all access

### Audit Logging

**What's Logged:**
- Bot commands (who, when, what)
- Configuration changes
- Verification attempts
- IP address access
- Cross-server data access

**Log Locations:**
- `bot.log` - Main log file
- `logs/` - Historical logs

---

## 🚀 Production Deployment

### Server Requirements

**Minimum:**
- Ubuntu 20.04+
- Python 3.11+
- 1GB RAM
- 10GB disk space

**Recommended:**
- Ubuntu 22.04 LTS
- Python 3.11+
- 2GB RAM
- 20GB disk space

### Installation Steps

**1. Update System:**
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3.11 python3.11-venv python3-pip git sqlite3 -y
```

**2. Create User:**
```bash
sudo useradd -r -s /bin/bash -m -d /opt/echobot echobot
sudo su - echobot
```

**3. Clone and Setup:**
```bash
git clone https://github.com/goldcyper/ns_echo_bot.git
cd ns_echo_bot
python3.11 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**4. Configure Bot:**
```bash
python cli/cli_setup.py token
python cli/cli_setup.py guild
```

**5. Setup Systemd Service:**

Create `/etc/systemd/system/echobot.service`:
```ini
[Unit]
Description=NationStates Echo Bot
After=network.target

[Service]
Type=simple
User=echobot
WorkingDirectory=/opt/echobot/ns_echo_bot
Environment="PATH=/opt/echobot/ns_echo_bot/venv/bin"
ExecStart=/opt/echobot/ns_echo_bot/venv/bin/python bot.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

**6. Enable & Start:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable echobot
sudo systemctl start echobot
```

### Monitoring

**Check Status:**
```bash
sudo systemctl status echobot
sudo journalctl -u echobot -f
```

**Restart:**
```bash
sudo systemctl restart echobot
```

### Backups

**Manual Backup:**
```bash
cp /opt/echobot/ns_echo_bot/echo.db /backup/echo.db.$(date +%Y%m%d)
```

**Database Backup Tool:**
```bash
python cli/cli_data.py backup
```

---

## 👨‍💻 Developer Guide

### Project Structure

```
ns_echo_bot/
├── bot.py                       # Main bot entry point
├── db_echo.py                   # Database operations
├── cli/                         # Command-line tools
│   ├── cli_setup.py             # Bot configuration
│   ├── cli_server.py            # Service management
│   └── cli_data.py              # Data management
├── handlers/                    # Command handlers
│   ├── commands.py              # General commands
│   ├── management.py            # Management commands
│   ├── handle_resident.py       # Resident commands
│   └── interactions.py          # Menu & interactions
├── modules/                     # Core functionality
│   ├── wa_proposals.py          # WA proposal tracking
│   ├── wa_update.py             # WA data updates
│   ├── ns_data_processing.py   # NS API processing
│   ├── telegram_operations.py   # Telegram automation
│   ├── rmb_listener.py          # RMB monitoring
│   └── werewolf/                # Werewolf game (optional)
├── utils/                       # Utility functions
├── lib/                         # Core libraries
│   ├── menus.py                 # Interactive menus
│   └── memory_tools.py          # Memory management
├── config/                      # Configuration & keys
├── data/                        # Runtime state
└── logs/                        # Log files
```

### Adding New Commands

**1. Create Handler:**
```python
# In handlers/your_handler.py
async def handle_your_command(ctx):
    """Your command description."""
    await ctx.send("Response")
```

**2. Register Command:**
```python
# In bot.py
@bot.command(name='yourcommand')
async def yourcommand(ctx):
    """Command help text"""
    await handle_your_command(ctx)
```

### Code Style

- PEP 8 compliance
- Type hints for parameters
- Docstrings for all functions
- Comment complex logic
- Use meaningful variable names

---

## 🐛 Troubleshooting

### Bot Won't Start

**Check Python version:**
```bash
python --version  # Need 3.11+
```

**Reinstall dependencies:**
```bash
pip install -r requirements.txt --force-reinstall
```

**Reset database:**
```bash
rm echo.db
python bot.py
```

**Check logs:**
```bash
cat bot.log
```

### Commands Not Working

**Check bot is online in Discord**

**Verify permissions:**
- Read Messages
- Send Messages
- Embed Links

**Check prefix:**
```
?guild_config
```

### Nation Verification Fails

- Check nation name spelling (use underscores)
- Verify within 2 minutes
- Copy full verification code

### Database Errors

**Check database integrity:**
```bash
sqlite3 echo.db "PRAGMA integrity_check;"
```

**Backup and reset:**
```bash
cp echo.db echo.db.backup
rm echo.db
python bot.py
```

---

## ❓ FAQ

**Q: Do I need to run separate services?**
A: No. The bot handles everything. You can run `python cli/cli_server.py both` for coordinated startup of both bot and API services.

**Q: Can I run on multiple Discord servers?**
A: Yes. Use the same bot token on all servers. Each server has independent configuration.

**Q: Is my data secure?**
A: Yes. All sensitive data is encrypted with AES-256. Keep `config/key.key` safe.

**Q: How do I backup data?**
A: Use `python cli/cli_data.py backup` to create a compressed backup.

**Q: Can I run on Windows?**
A: Yes. All commands work in PowerShell. The bot is cross-platform.

**Q: Where are logs stored?**
A: Main: `bot.log`. Historical: `logs/` directory. Systemd: `journalctl -u echobot`.

---

## 🐺 Werewolf Game Module (Optional)

The project includes an optional Werewolf game engine for forum-based games.

### Overview
- Forum-based Werewolf game using XenForo
- Automates signup, role assignment, phases, voting
- Special roles: Big Bad, Hero of Resistance, Recruiter, Counter-Intelligence

### Components
- `modules/werewolf/` - Game logic and role handling
- `modules/werewolf/signup.txt` - Signup template
- `modules/werewolf/werewolf_schema.sql` - Database schema

### Status
- Optional/advanced feature
- Core bot works fine without it
- Requires XenForo forum setup

---

## 📞 Support

**Need Help?**

- 📖 [GitHub Documentation](https://github.com/goldcyper/ns_echo_bot)
- 🐛 [Report Bugs](https://github.com/goldcyper/ns_echo_bot/issues)
- 💬 [Ask Questions](https://github.com/goldcyper/ns_echo_bot/discussions)
- ⭐ Star the repo!

---

## 📝 License

MIT License - See [LICENSE](LICENSE) file for details.

---

**Last Updated:** February 27, 2026  
**Version:** 2.0  
**Maintained by:** goldcyper

Made with ❤️ for the NationStates community
