<div align="center">

# 🌍 NS Echo Bot

### **The Ultimate NationStates Discord Bot for Region Management & Automation**

[![Python Version](https://img.shields.io/badge/python-3.11%2B-blue.svg)](https://www.python.org/downloads/)
[![Discord.py](https://img.shields.io/badge/discord.py-2.3%2B-blue.svg)](https://github.com/Rapptz/discord.py)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-active-success.svg)]()

**[Features](#-key-features) • [Quick Start](#-quick-start) • [Documentation](#-table-of-contents) • [Support](#-support)**

---

### 🎯 What is NS Echo Bot?

A powerful, multi-guild Discord bot built for NationStates communities. Automate telegram campaigns, track WA endorsements, manage residents, process citizenship applications, and monitor your regions—all from Discord!

### 🌟 Why Choose Echo Bot?

✨ **Multi-Server Support** - Manage multiple Discord servers with isolated configurations  
🔐 **Enterprise Security** - AES-256 encryption for all sensitive data  
🤖 **Telegram Automation** - Automated recruitment, endorsement, and restoration campaigns  
📊 **Real-Time NS Data** - Live integration with NationStates API  
👥 **Resident Management** - Track nations across regions with privacy controls  
🎓 **Citizenship System** - Automated applications with security screening  
⚡ **High Performance** - Optimized for large regions and high traffic  

</div>

---

## 📖 Table of Contents

### 🚀 Getting Started
- [Quick Start](#-quick-start)
- [Installation](#installation)
- [First Run](#first-run)
- [Key Features](#-key-features)

### ⚙️ Configuration
- [CLI Tools Reference](#-cli-tools-reference)
- [Configuration Guide](#-configuration-guide)
- [Multi-Server Setup](#-multi-server-setup)

### 📝 Commands & Features
- [Commands Reference](#-commands-reference)
- [Telegram Automation](#-telegram-automation)
- [Citizenship Applications](#-citizenship-applications)
- [Resident Management](#-resident-management)
- [World Assembly Features](#-world-assembly-features)

### 🔒 Security & Deployment
- [Security & Privacy](#-security--privacy)
- [Production Deployment](#-production-deployment)
- [Backup & Recovery](#backup--recovery)

### 👨‍💻 Advanced
- [Developer Guide](#-developer-guide)
- [API Reference](#api-reference)
- [Troubleshooting](#-troubleshooting)
- [FAQ](#-faq)

---

## 🚀 Quick Start

### ⚡ Installation

**Step 1: Clone the Repository**
```bash
git clone https://github.com/goldcyper/ns_echo_bot.git
cd ns_echo_bot
```

**Step 2: Install Dependencies**
```bash
pip install -r requirements.txt
```

> 💡 **Tip:** Using a virtual environment is recommended!
> ```bash
> p✅ Check and install dependencies automatically
2. 🔑 Prompt for your Discord bot token
3. 💾 Initialize the database
4. 🌐 Start the verification server
5. 🎊 Be ready to use!

### 🛠️
### 🎉 First Run

```bash
python bot.py
```

On first run, the bot will:
1. Check and install dependencies automatically
2. Prompt for your Discord bot token
3. Initialize the database
4. Start the verification server

### Initial Setup via CLI

```bash
# 1. Setup bot token
python cli/cli_setup.py token

# 2. Configure guild settings
python cli/cli_setup.py guild
```
💬 Basic Setup in Discord

In your Discord server, type:

```
?menu
```

Then configure in this order:

1. 👤 **Bot Permissions** - Add yourself as admin
2. 🔐 **X-Auto-Login** - Connect your NationStates account
3. 📺 **Channels** - Set up WA and log channels
4. 🌍 **Region** - Configure your region

### 📦
### Requirements
**Required:**
- 🐍 Python 3.11 or higher
- 💬 discord.py >= 2.3.0
- 🌐 requests >= 2.31.0
- 💻 psutil >= 5.9.0
- 🔐 cryptography >= 41.0.0
- 📄 lxml >= 4.9.0
- ⚡ aiohttp >= 3.8.0

### ✨
### Key Features
🌐 **Multi-Guild Support** - Manage multiple Discord servers with isolated configurations  
📡 **NationStates Integration** - Real-time data from NS API with intelligent caching  
📨 **Telegram Automation** - Automated recruitment, endorsement & restoration campaigns  
🏛️ **WA Tracking** - Monitor WA nations, endorsements, and voting patterns  
👥 **Resident Management** - Track and manage region residents with privacy controls  
🎓 **Citizenship Applications** - Automated applications with security screening & proxy detection  
📊 **Region History** - Track nation movements and region history over time  
🔔 **WA Proposals & Resolutions** - Auto-post new proposals and resolutions to Discord  
🔐 **Enterprise Security** - AES-256 encryption, IP hashing, and audit logging  
⚡ **High Performance** - Optimized memory usage and concurrent processing  
🛠️ CLI Tools Reference

### 📋 Overview

The bot includes three powerful CLI tools located in the `cli/` directory:

| Tool | Purpose | Use When |
|------|---------|----------|
| 🔧 **cli_setup.py** | Bot configuration & credentials | Setting up new servers or updating settings |
| 🚀 **cli_server.py** | Service management & monitoring | Starting, stopping, or checking bot status |
| 💾 **cli_data.py** | Database operations & imports | Importing data, backups, or managing residents |omated application system

---

## CLI Tools Reference

### Overview

The bot includes three main CLI tools located in the `cli/` directory:

1. **cli_setup.py** - Bot configuration
2. **cli_server.py** - Server management
3. *🔧 cli_setup.py - Bot Configuration

Configure bot settings and guild preferences securely.

#### 🔑igure bot settings and guild preferences.

#### Setup Bot Token

```bash
python cli/cli_setup.py token
```
✨ **Features:**
- 🔐 Prompts for Discord bot token
- 🔒 Encrypts and stores token securely using AES-256
- ✅ Validates token format before saving
- 💾 Stores in encrypted database

#### ⚙️ Setup Guild Configuration

```bash
📝 **Interactive menu for:**
- 🌍 Add/remove guild regions
- 🔐 Configure API credentials (nation/password)
- ✅ Enable/disable regions for monitoring
- 📧 Set contact consent settings
- 🎨 Customize per-guild settings

#### ❓t contact consent settings

#### Help

```bash
python cli/cli_setup.py --help
```
🚀 cli_server.py - Server Management

Start, stop, and monitor bot services with ease.

#### 💬
Start and manage bot services.

#### Start Discord Bot Only

```bash
python cli/cli_server.py bot
✨ **What happens:**
- 🤖 Starts Discord bot service
- 📡 Bot will auto-start Echo API if needed
- 🔄 Automatic reconnection on errors
- ⏹️ Press Ctrl+C to stop gracefully

#### 📡
#### Start Echo API Only

```bash
✨ **Features:**
- 📊 Starts Echo API data processing service
- 🌍 Handles NationStates daily dumps
- 💾 Updates nation/region history database
- ⏹️ Press Ctrl+C to stop

#### 🚀ess Ctrl+C to stop

#### Start Both Services
✨ **Coordinated startup:**
- 1️⃣ Starts Echo API first (waits for ready)
- 2️⃣ Then starts Discord bot
- 🏥 Health checks ensure proper startup
- 🔄 Both services restart together on errors
- ⏹️ Press Ctrl+C to stop all services gracefully

#### 📊en starts Discord bot
- Coordinated startup with health checks
- Press Ctrl+C to stop all services
📋 **Shows:**
- 🟢 Running status of all services
- 🆔 Process IDs (PIDs)
- 🏥 API health information
- 💾 Database status
- 📊 Memory usage

---

### 💾 cli_data.py - Data Management

Manage database imports, exports, backups, and resident data like a pro!

#### 📥

### cli_data.py - Data Management

✨ **Features:**
- 📦 Default: `echo_api_deployment.db.gz`
- 🗜️ Automatic decompression
- 💾 Imports complete database
- ✅ Verifies integrity after import
- 📊 Shows table statistics and row counts

#### 📄on cli/cli_data.py import [backup_file]
```
- Default: `echo_api_deployment.db.gz`
- Decompresses and imports database
- Verifies integrity after import
✨ **Perfect for GitHub deployments:**
- 📁 Imports from `echo_api_deploy/` directory
- 1️⃣ Applies schema first, then data parts sequentially
- ⏱️ Takes 10-20 minutes depending on system
- 📊 Progress shown for each part file
- 🔄 Automatic error recovery
```bash
pytho📤 Export to SQL Shards

```bash
python cli/cli_data.py export
```
✨ **For GitHub/version control:**
- 💾 Exports `echo_api.db` to SQL shards
- 📁 Creates files in `echo_api_deploy/` directory
- ✂️ Splits data into 24MB chunks (GitHub file size limit)
- 📋 Generates schema and index files separately
- 🔢 Creates numbered part files (part_001, part_002, etc.)

#### 📦on cli/cli_data.py export
```
- Exports `echo_api.db` to SQL shards
✨ **Full backup creation:**
- 📦 Creates `echo_api_deployment.db.gz`
- 🚀 Optimizes database before compression (VACUUM)
- ✅ Verifies integrity after compression
- 📝 Includes metadata file with timestamp
- 🗜️ High compression ratio (typically 10:1)

#### 👥
```bash
python cli/cli_data.py backup
```
- Creates `echo_api_deployment.db.gz`
- Optimizes database before compression
✨ **Flexible import:**
- 📄 Imports resident data from local CSV file
- 📊 Supports Google Sheets URLs (auto-converts to CSV)
- ➕ Creates new nation records automatically
- 🔄 Updates existing resident information
- 📈 Shows detailed import statistics
- 🔐 Encrypts IP addresses automatically

**CSV Format (Required)
python cli/cli_data.py residents <file_or_url>
```
- Imports resident data from CSV file
- Supports Google Sheets URLs (auto-converts to CSV)
- 💡 Google Sheets Example:**
```bash
python cli/cli_data.py residents "https://docs.google.com/spreadsheets/d/ABC123/edit"
```

> **Tip:** The bot automatically converts Google Sheets to CSV format!

#### 🌍on,discord_id,citizen,admin_permission,contact_consent,opt_in_telegram
nation_name,123456789,yes,no,yes,no
```

**Google Sheets Example:**
✨ **Auto-populate residents:**
- 🌍 Fetches data from all enabled regions
- 🏛️ Processes WA nations automatically
- 🤝 Updates resident endorsement counts
- 📊 Shows before/after statistics
- 🔄 Can be run repeatedly for updates

---

### 🎯 Common CLI Workflows

> **These are the most common task sequences you'll use!**

#### 🆕pulates residents table from enabled regions
- Processes WA nations data
- Updates resident endorsement counts
- Shows before/after statistics

---

### Common CLI Workflows

#### First Time Setup
```bash
# 1. Setup bot token
python cli/cli_setup.py token

# 2. Import database (choose one method)
python cli/cli_data.py import echo_api_deployment.db.gz
# OR🚀 
python cli/cli_data.py import-sql

# 3. Configure guild
python cli/cli_setup.py guild

# 4. Import residents (optional)
python cli/cli_data.py residents residents.csv

# 5. Start bot
python bot.py
```👨‍💻 

#### Regular Deployment
```bash
# Update database
python cli/cli_data.py import echo_api_deployment.db.gz

# Start services
python cli/cli_server.py both
```

#### Development Workflow
```bash
# Start API in one terminal
python cli/cli_server.py api

# S📝 Commands Reference

### 🎮
# Check status
python cli/cli_server.py status Admin Only |
|---------|-------------|---------|:----------:|
| `?menu` | 🎛️ Interactive configuration menu | `?menu` | ✅ |
| `?guild_config` | ⚙️ Show server configuration | `?guild_config` | ✅ |
| `?health` | 🏥 Check bot status & uptime | `?health` | ❌ |
| `?help` | ❓ Show all commands | `?help` | ❌ |

### 🔐 Core Commands

| Command | Description | Example |
|---------|-------------|---------|
| `?menu` | Interactive configuration menu | `?menu` |
| `?guild_config` | Show server con Admin Only |
|---------|-------------|---------|:----------:|
| `?verify <nation>` | ✅ Verify nation ownership | `?verify Test_Nation` | ❌ |
| `?whois <user/nation>` | 🔍 Check verification status | `?whois @User` or `?whois Nation` | ❌ |
| `?unverify <user>` | ❌ Remove verification | `?unverify @User` | ✅ |

### 🏛️ommand | Description | Example |
|---------|-------------|---------|
| `?verify <nation>` | Verify nation ownership | `?verify Test_Nation` |
| `?whois <user/nation>` | Check verification status | `?whois @User` or `?whois Nation` |
| `?unverify <user>` | Remove verif Admin Only |
|---------|-------------|---------|:----------:|
| `?newnations [region]` | 🆕 List new WA nations in region | `?newnations Lazarus` | ❌ |
| `?wanations <region>` | 🌍 List all WA nations | `?wanations Lazarus` | ❌ |
| `?ga [id]` | 🏛️ Show GA resolution (current or by ID) | `?ga` or `?ga 123` | ❌ |
| `?sc [id]` | 🛡️ Show SC resolution (current or by ID) | `?sc` or `?sc 456` | ❌ |
| `?proposals` | 📋 Show active WA proposals | `?proposals` | ❌ |
| `?check_proposals` | 🔄 Manually check for new proposals | `?check_proposals` | ✅ |
 Admin Only |
|---------|-------------|---------|:----------:|
| `?listresident <search>` | 🔍 Search residents (current server only) | `?listresident john` | ✅ |
| `?residents <search>` | 🌐 Search all authorized servers | `?residents john` | ✅ |
| `?addresident <nation>` | ➕ Add resident manually | `?addresident Nation_Name` | ✅ |

### 🎓
| Command | Description | Example |
|---------|-------------|---------|
| `?listresident <search>` | Search residents (current server) | `?listresident john` |
| `?residents <search>` | Search all authorized servers | `?residents john` |
| `?addresident <nation>` | Add resident manually | `?addresident Nation_Name` |
 Admin Only |
|---------|-------------|---------|:----------:|
| `?citizenship` | 📝 Start citizenship application | `?citizenship` | ❌ |
| `?applications` | 📋 View pending applications | `?applications` | ✅ |

### 📨?citizenship` | Start application | `?citizenship` |
| `?applications` | View pending applications | `?applications` |
 Admin Only |
|---------|-------------|---------|:----------:|
| `?telegram_status` | 📊 Check automation status & stats | `?telegram_status` | ✅ |

---

### 🌍 World Assembly Features

Echo Bot includes comprehensive WA tracking and automation:

#### 📊 Automatic Monitoring
- ✅ **Resolutions** - Auto-post new GA/SC resolutions to Discord
- ✅ **Proposals** - Track proposals seeking quorum (checks every 15 minutes)
- ✅ **Voting Stats** - Display vote percentages and delegate voting
- ✅ **Historical Lookup** - Query past resolutions by ID

#### 🏛️ Resolution Commands

**View Current Resolution:**
```
?ga          # General Assembly current resolution
?sc          # Security Council current resolution
```

**View Historical Resolution:**
```
?ga 123      # GA resolution #123
?sc 456      # SC resolution #456
```

**Display Options:**
```
?ga text                  # Show full resolution text
?ga votes                 # Show voting statistics
?ga delegates             # Show top 10 delegate votes
?ga nations               # Show nation vote counts
?ga text votes delegates  # Combine multiple options
```

#### 📋 Proposal Tracking

**What Gets Posted:**
- ✅ GA proposals with ≥56 approvals + legal decision
- ✅ SC proposals with ≥1 approval + legal decision
- ✅ Creates individual Discord threads for each proposal
- ✅ Full proposal details in thread, summary in channel
- ✅ Tracks posted proposals per guild (no duplicates)

**Manual Check:**
```
?check_proposals  # Force immediate proposal check
```
| Command | Description | Example |
|---------|-------------|---------|
| `?telegram_status` | Check automation status | `?telegram_status` |

---

## 🌐 Multi-Server Setup

### 📋 Overview

Run Echo Bot on multiple Discord servers simultaneously, each with:
- 🌍 Different NationStates regions
- 🔐 Separate credentials (nation/password)
- ⚙️ Independent configurations
- 💾 Isolated data per server

### 🏗️ Architecture

```
Bot Instance
├── 🏛️ Server A (Lazarus)
│   ├── Region: Lazarus
│   ├── Credentials: Nation_A + Password_A
│   └── Settings: Independent config
└── 🌊 Server B (The Pacific)
    ├── Region: The Pacific
    ├── Credentials: Nation_B + Password_B
    └── Settings: Independent config
```

### 🚀 Setup Steps

#### 1️⃣ Invite Bot to Multiple Servers

Use the same Discord bot token for all servers.

> **💡 Pro Tip:** Generate your bot invite link at [Discord Developer Portal](https://discord.com/developers/applications)

**On Server A (Lazarus):**
```
?menu
→ 🔧 Setup & Settings
→ 🔐 Setup X-Auto-Login (enter Nation_A credentials)
→ 🌍 Region Management (add and enable Lazarus)
```

**On Server B (The Pacific):**
```
?menu
→ 🔧 Setup & Settings
→ 🔐 Setup X-Auto-Login (enter Nation_B credentials)
→ 🌍 Region Management (add and enable The Pacific)
```

#### 3️⃣

#### 3. Verify Isolation

Run in each server:
```
?guild_config
```

Each server will show its own:
- 🌍 Region configuration
- 🔐 Credentials (nation name only, password hidden)
- 📺 Channel settings  
- ✅ Verification data
- 📊 Independent statistics

### 🤝 Data Sharing

#### 👥 Resident Database Sharing

Allow specific servers to access your resident data:
👥 Residents Management → 🔓 Authorize Guild Access
```

**🔒 Privacy Controls:**
- 🔐 IP addresses only visible to origin server admins
- 👀 Other servers see nation names and basic info only
- ❌ Access can be revoked anytime
- 📝 Audit logging for all access

#### 🌐cess can be revoked anytime

#### Cross-Server Search

Users with permissions can search across authorized servers:

```
**📊 Results show:**
- 🏷️ Nation names
- 💬 Discord usernames (if verified)
- 🏠 Server origin
- 📋 Basic resident info
- ✅ Verification status

---

## ⚙️ Configuration Guide

### 🎛️
## Configuration Guide

### Menu Systemconfiguration menu:

```
?menu
```

> **💡 Tip:** The menu system is the easiest way to configure the bot! All settings can be managed through interactive prompts.

### 📂 Main Sections

#### 🔧 1. Setup & Settings

**👤 Bot Permissions**
- ➕ Add/remove admin users
- 🎮 Control who can configure the bot
- 🔒 Server-specific admin lists

**🔐 X-Auto-Login**
- 🔗 Connect NationStates account
- 🔒 AES-256 encrypted credential storage
- 🌍 Multiple nations supported per server
- 🔄 Update credentials anytime

**🎨 Change Command Prefix**
- 📝 Default: `?`
- ✏️ Change to `!`, `.`, `~`, or any custom prefix
- 💬 Avoid conflicts with other bots

**🛡️ Proxy Check API Key**
- 🔐 For citizenship application security
- 🆓 Get free key from [proxycheck.io](https://proxycheck.io)
- 🌐 Detects VPNs, proxies, and suspicious IPs

#### 📺 2. Channel Setup

**📋 Log Channel**
- 🤖 Bot activity logs
- ⚠️ Error notifications
- 📊 Command usage tracking

**🏛️ WA Resolutions Channel**
- 📢 New WA proposals
- 🗳️ Voting updates
- ✅ Resolution results
- 📊 Voting statistics

**🆕 New WA Nations Channel**
- 🎉 Notifications when nations join WA
- 🌍 Region-specific filtering
- 📈 Growth tracking

**🧵 WA Thread Settings**
- 🔄 Auto-create threads for resolutions
- 🎨 Custom thread names with `{date}` / `{time}` placeholders
- 📅 Auto-archive duration (1 hour to 1 week)
- 💬 Keeps discussions organized

#### 🌍 3. Region Management

**➕ Add/Enable Regions**
- 🌐 Monitor multiple regions simultaneously
- ✅ Enable/disable tracking per region
- 🔄 Real-time data updates
- 📧 Recruitment**
- 👋 Welcome new nations to region
- 🤖 Auto-send on nation creation
- 🎯 Target specific criteria

**🔄 Restorer**
- 🎯 Target nations that recently moved
- 📢 Re-recruitment campaigns
- ⏱️ Customizable timing

**🤝 Endorsements**
- 💬 Request endorsements from WA members
- 📈 Boost delegate influence
- 🎯 Smart targeting

**⚠️ Over Cap**
- 🛡️ Ask nations to reduce endorsements
- 🔐 Security Council enforcement
- 🚨 Automatic monitoring

**⚙️ Each type includes:**
- ✅ Enable/disable toggle
- ✏️ Custom message templates with variables
- ⏰ Scheduling options
- 🎯 Target filters and conditions
- 📊 Daily send limits
- 📈 Statistics tracking

#### 🎓 5. Citizenship Applications

**📝 Transcript Channel**
- 📋 Where applications are posted
- 👥 Admin review and voting
- 🔒 Private and secure
- 📊 Application statistics

**❓ Question Management**
- ✏️ Customize application questions
- ➡️ Add conditional questions
- 📊 Multiple choice or text responses
- 🔢 Unlimited questions

**🛡️ Proxy Verification**
- 🤖 Automatic VPN/proxy detection
- 🌐 IP address security checks
- 🔍 Resident IP matching
- 📊 Security scoring

#### 👥 6. Residents Management

**📥 Import from CSV/Google Sheets**
- 📊 Bulk import resident data
- 🔗 Direct Google Sheets integration
- 🔄 Update existing records
- ➕ Create new entries

**🤝 Cross-Guild Access**
- 🔓 Share residents with other servers
- 🔒 Privacy controls per guild
- 📝 Audit loggingkey purposes:

| Type | Purpose | Icon |
|------|---------|:----:|
| **Recruitment** | Welcome new nations | 👋 |
| **Restorer** | Re-recruit nations that left | 🔄 |
| **Endorsements** | Request endorsement swaps | 🤝 |
| **Over Cap** | Ask nations to reduce endorsements | ⚠️ |

### ⚙️ Generate reports
- 📋 Batch operations

---

## 📨 Telegram Automation

### 📋ee [Resident Management](#resident-management) section

**Cross-Guild Access**
- Share residents with other servers
- Privacy controls
📨 Telegram Automation

### 👋 Recruitment Telegrams

**🎯 
---

## Telegram Automation

### Overview

Automate NationStates telegram campaigns for four purposes:
1. **Recruitment** - Welcome new nations
2. **Restorer** - Re-recruit nations that left
3. **Endorsements** - Request endorsement swaps
4. **Over Cap** - Ask nations to reduce endorsements

### Configuration

Access via `?menu` → Telegram Automation

### Recruitment Telegrams
⚙️ Setup:**
1. ✅ Enable recruitment automation
2. ✏️ Set custom welcome message (BBCode supported)
3. ⏰ Configure send schedule (daily recommended)
4. 🌍 Set target region
5. 🎯 Optional: Add filters (WA only, age, etc.)

**📝  Configure send schedule (daily recommended)
👋 Welcome to [region]Lazarus[/region], @@NATION@@!

We're glad you're here. Check out our [dispatch=123456]📚 Regional Guide[/dispatch] to get started.

Join our [url=discord.gg/example]💬 Discord server[/url] for community chat!

Looking forward to seeing you around! 🌟
```

**🔧 Variables:**
- `@@NATION@@` - Replaced with recipient's nation name
- `[region]...[/region]` - Creates NS region link
- `[dispatch=ID]...[/dispatch]` - Links to dispatch
- `[url=...]...[/url]` - External links

### 🔄 Restorer Telegrams

**🎯 `[url=...]...[/url]` - External links

### Restorer Telegrams
⚙️ Setup:**
1. ✅ Enable restorer automation
2. ✏️ Set custom message
3. 📅 Configure minimum days since move (recommended: 7-30 days)
4. 🎯 Set maximum targets per day
5. 🔍 Optional: Filter by previous region

**💡 Use Case:** Nations that left your region might return if reminded of what they're missing!

### 🤝 Endorsement Telegrams

**🎯 
##⚙️ Setup:**
1. ✅ Enable endorsement automation
2. 🏷️ Set your nation (delegate or candidate)
3. ✏️ Configure message
4. 🎯 Set daily send limit
5. 🔍 Filter: Only target WA members who haven't endorsed you

**📊 Smart Targeting:**
- ❌ Skips nations that already endorsed you
- ✅ Only targets WA members in your region
- 🔄 Updates endorsement list automatically

##⚙️ Setup:**
1. ✅ Enable over cap automation
2. 📊 Set endorsement cap limit (e.g., 50% of delegate's endorsements)
3. ✏️ Configure message
4. 🤖 Automatic targeting of nations over limit
5. 🔔 Optional: Alert admins when triggered

**🛡️ Security Use Case:** Prevent influence coups and maintain regional security

### ⏱️ Rate Limiting

All telegram types respect NS API rules:
- ⏰ 10 second cooldown between telegrams
- 🤖 Automatic rate limiting
- 📊 Daily send limits configurable
- 🔒 API compliance enforced

### 📊
### Rate Limiting

All telegram types respect NS API rules:
- 10 second cooldown between telegrams
- Automatic rate limiting
- Daily send limits configurable
**📈 Shows:**
- ✅ Enabled telegram types
- 🕐 Last send times
- 📊 Daily counts and limits
- 📋 Queue status
- 🎯 Next scheduled runs

---

## 🎓 Citizenship Applications

### 📋ast send times
- Daily counts
- Queue status

---

## Citizenship Applications

### Overview

Automated citizenship application system with:
- Custom questionnaires
- Nation verification
- Proxy/VPN detection
- IP security checks
- Discord integration

### Setup

#### 1. Configure Transcript Channel

```
?menu → Citizenship Applications → Setup Transcript Channel
```

This is where applications are posted for admin review.

#### 2. Setup Proxy Check (Optional but Recommended)

Get a free API key from https://proxycheck.io

```
?menu → Setup & Settings → Setup Proxy Check API Key
```

#### 3. Customize Questions

```
?menu → Citizenship Applications → Manage Questions
```

**Default Questions:**
1. What is your main NationStates nation?
2. Why do you want to join our region?
3. Have you been a member of any other regions?

**Question Types:**
- **Text Input** - Free-form responses
- **Multiple Choice** - Pre-defined options
- **Conditional** - Show based on previous answers

### Application Process

#### User Experience

1. User types `?citizenship` in Discord
2. Bot sends DM with questionnaire
3. User answers questions in DMs
4. Bot sends verification link
5. User clicks link, verifies nation ownership
6. Application posted to transcript channel

#### Verification Link

The link performs:
- **Nation Verification** - Confirms user owns the nation
- **IP Capture** - Records IP address (encrypted)
- **Proxy Check** - Detects VPNs/proxies (if API key configured)
- **Resident Matching** - Checks if IP matches existing residents

#### Admin Review

Application appears in transcript channel with security information and approve/deny buttons.

### Privacy & Security

**Data Encryption:**
- All IP addresses encrypted with AES-256
- IP hashes stored for matching
- Original IPs only visible to admins

**Data Retention:**
- Applications stored indefinitely (configurable)
- IP data only accessible by origin server admins
- Delete on request

**Access Control:**
- Only admins can view full applications
- Regular users cannot access IP data
- Audit logging for all access

---

## Resident Management

### Overview

Track and manage residents across your NationStates regions with:
- Bulk import from CSV/Google Sheets
- Cross-server search
- IP address tracking
- Ban management
- Privacy controls

### Resident Import

#### From CSV File

**Required Columns:**
- `nation` - NationStates nation name (underscores for spaces)

**Optional Columns:**
- `discord_username` - Discord username
- `discord_id` - Discord user ID (for verification matching)
- `ip_address` - IP address (will be encrypted)
- `notes` - Admin notes
- `banned` - 0 or 1

**Import via CLI:**
```bash
python cli/cli_data.py residents residents_template.csv
```

#### From Google Sheets

```bash
python cli/cli_data.py residents "https://docs.google.com/spreadsheets/d/SHEET_ID/edit"
```

### Searching Residents

#### Local Search (Current Server Only)

```
?listresident john
```

Returns:
- Nation names matching "john"
- Discord usernames
- Verification status
- Ban status
- IP addresses (admins only)
- Notes

#### Cross-Server Search

```
?residents john
```

Searches all servers that have authorized your server.

### Privacy Controls

**What's Shared in Cross-Server Search:**
- Nation names
- Discord usernames (if not sensitive)
- Verification status
- Basic info

**What's NOT Shared:**
- IP addresses (origin server admins only)
- Personal notes
- Ban reasons (unless public)

**Revoking Access:**
```
?menu → Residents Management → Revoke Guild Access
```

---

## Security & Privacy

### Encryption

**What's Encrypted:**
- Discord bot token (AES-256)
- NationStates passwords (AES-256)
- IP addresses (AES-256)
- API keys (AES-256)
- Verification tokens (Fernet)

**Encryption Key:**
- Generated on first run
- Stored in `config/key.key`
- **Keep this file secure!**
- Loss of key = loss of encrypted data

### IP Address Security

**Storage:**
- IP addresses encrypted before storage
- SHA-256 hash stored for matching
- Prefix hash (first 3 octets) for subnet matching

**Access:**
- Only admins can view IPs
- Only admins of the origin server
- Audit logging for all IP access

### Audit Logging

**What's Logged:**
- Bot commands (who, when, what)
- Configuration changes
- Verification attempts
- IP address access
- Ban/unban actions
- Cross-server data access

**Log Location:**
- `bot.log` - Main log file
- `logs/` directory - Historical logs

---

## Production Deployment

### Server Requirements

**Minimum:**
- Ubuntu 20.04 or newer
- Python 3.11+
- 1GB RAM
- 10GB disk space

**Recommended:**
- Ubuntu 22.04 LTS
- Python 3.11+
- 2GB RAM
- 20GB disk space
- Static IP (for verification server)

### Installation

#### 1. System Setup

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Python 3.11
sudo apt install python3.11 python3.11-venv python3-pip -y

# Install system dependencies
sudo apt install git sqlite3 -y
```

#### 2. Create User

```bash
# Create dedicated user
sudo useradd -r -s /bin/bash -m -d /opt/echobot echobot

# Switch to user
sudo su - echobot
```

#### 3. Clone and Setup

```bash
# Clone repository
git clone https://github.com/goldcyper/ns_echo_bot.git
cd ns_echo_bot

# Create virtual environment
python3.11 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

#### 4. Configure Bot

```bash
# Run initial setup
python bot.py
# Enter Discord bot token when prompted
# Bot will create database and encrypt token
# Press Ctrl+C after successful start
```

#### 5. Setup Systemd Service

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

Enable and start:

```bash
sudo systemctl daemon-reload
sudo systemctl enable echobot
sudo systemctl start echobot
```

### Monitoring

**Check Status:**
```bash
sudo systemctl status echobot
```

**View Logs:**
```bash
# Live logs
sudo journalctl -u echobot -f

# Last 100 lines
sudo journalctl -u echobot -n 100
```

**Restart Bot:**
```bash
sudo systemctl restart echobot
```

### Backup

**Database Backup:**
```bash
# Manual backup
cp /opt/echobot/ns_echo_bot/echo.db /backup/echo.db.$(date +%Y%m%d)

# Automated daily backup (crontab)
0 2 * * * cp /opt/echobot/ns_echo_bot/echo.db /backup/echo.db.$(date +\%Y\%m\%d)
```

### Updates

**Update Bot:**
```bash
# Stop bot
sudo systemctl stop echobot

# Switch to bot user
sudo su - echobot
cd ns_echo_bot

# Backup database first
cp echo.db echo.db.backup

# Pull updates
git pull

# Activate venv and update dependencies
source venv/bin/activate
pip install -r requirements.txt --upgrade

# Exit to root
exit

# Start bot
sudo systemctl start echobot
```

---

## Developer Guide

### Project Structure

```
ns_echo_bot/
├── bot.py                   # Main bot entry point
├── echo_api.py              # NationStates API service
├── db_echo.py               # Database operations
├── cli/                     # Command-line tools
│   ├── cli_setup.py         # Configuration
│   ├── cli_server.py        # Server management
│   └── cli_data.py          # Data management
├── handlers/                # Command handlers
│   ├── config_handlers.py   # Configuration commands
│   ├── wa_commands.py       # WA-related commands
│   ├── region_handlers.py   # Region management
│   └── ...
├── modules/                 # Core functionality
│   ├── auto_telegram.py     # Telegram automation
│   ├── wa_update.py         # WA data updates
│   ├── ns_data_processing.py # NS API helpers
│   └── ...
├── utils/                   # Utility functions
│   ├── database/            # DB utilities
│   ├── nationstates/        # NS API wrappers
│   └── discord/             # Discord helpers
├── lib/                     # Core library modules
│   ├── menus.py             # Interactive menu system
│   ├── database_coordinator.py # Database coordination
│   ├── echo_api_status.py   # API status management
│   ├── memory_tools.py      # Memory monitoring
│   ├── proxy_verification_server.py # Verification server
│   ├── simple_server.py     # HTTP server
│   └── peak_processing_optimizer.py # Performance optimizer
├── config/                  # Configuration & credentials
│   ├── config.json          # Bot configuration
│   ├── key.key              # Encryption key
│   ├── token_key.key        # Token encryption key
│   └── verification_tokens.enc # Verification tokens
├── data/                    # Runtime state data
│   └── echo_api_status.json # API status tracking
├── scripts/                 # Utility scripts
│   ├── tools.py             # Diagnostic CLI
│   ├── analyze_memory_log.ps1
│   └── install_nodejs.sh
├── templates/               # CSV templates
│   └── residents_template.csv
└── assets/                  # Images and assets
    └── bot.png
```

### Database Schema

**Key Tables:**
- `setup` - Bot configuration per guild
- `guild_regions` - Guild-to-region mapping
- `guild_ns_credentials` - Encrypted NS credentials per guild
- `verified_nations` - Nation verification records
- `residents` - Resident database
- `citizenship_applications` - Application records
- `resolutions` - WA resolutions
- `nations` - Nation data cache
- `regions` - Region data cache

### Adding New Commands

**1. Create Handler Function:**

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

**Follow:**
- PEP 8 style guide
- Type hints for function parameters
- Docstrings for all functions
- Comment complex logic
- Use meaningful variable names

### Contributing

**Process:**
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Make changes and test
4. Commit: `git commit -m "Add your feature"`
5. Push: `git push origin feature/your-feature`
6. Open Pull Request

---

## Troubleshooting

### Bot Won't Start

**Problem:** Bot crashes on startup

**Solutions:**
1. Check Python version: `python --version` (need 3.11+)
2. Reinstall dependencies: `pip install -r requirements.txt --force-reinstall`
3. Check bot token is valid
4. Delete `echo.db` and restart (resets database)
5. Check logs in `bot.log`

### Commands Not Working

**Problem:** Bot doesn't respond to commands

**Solutions:**
1. Check bot is online in Discord
2. Verify bot has correct permissions:
   - Read Messages
   - Send Messages
   - Embed Links
   - Attach Files
   - Add Reactions
3. Check command prefix: `?guild_config`
4. Try `?help` to see if bot responds

### Verification Fails

**Problem:** Nation verification doesn't work

**Solutions:**
1. Check nation name spelling (use underscores)
2. Verify code within 2 minutes
3. Make sure you're copying full code from NS
4. Check verification server is running: `http://your-ip:4116/health`
5. Try `?verify` again

### Database Errors

**Problem:** Database-related errors

**Solutions:**
1. Check `echo.db` file isn't corrupted: `sqlite3 echo.db "PRAGMA integrity_check;"`
2. Backup and reset: `cp echo.db echo.db.backup && rm echo.db && python bot.py`
3. Check disk space: `df -h`
4. Check file permissions: `ls -l echo.db`

### Import Errors

**Problem:** Resident import fails

**Solutions:**
1. Check CSV format matches template
2. Verify all required columns present
3. Check for special characters in data
4. Ensure IP addresses are valid (if provided)
5. Try smaller batches (split CSV)

---

## FAQ

**Q: Do I need to run echo_api.py separately?**
A: No. The bot auto-starts the Echo API service when needed. You can also use `python cli/cli_server.py both` for coordinated startup.

**Q: Can I run multiple bots?**
A: Yes, but each bot needs its own Discord bot token and database file.

**Q: Is my data secure?**
A: Yes. All sensitive data is encrypted with AES-256. Keep your `config/key.key` file secure.

**Q: How do I backup my data?**
A: Use `python cli/cli_data.py backup` or copy `echo.db`, `config/key.key`, and `config/verification_tokens.enc` files.

**Q: What if I lose my encryption key?**
A: You'll lose access to encrypted data (passwords, IPs). Keep `config/key.key` backed up securely.

**Q: Can I run this on Windows?**
A: Yes. All commands work in PowerShell. The bot is cross-platform.

**Q: How do I update the bot?**
A: `git pull` to get updates, then restart the bot. Database migrates automatically.

**Q: Where are logs stored?**
A: Main log: `bot.log`. Memory logs: `logs/`. Systemd: `journalctl -u echobot`.

---

## Support

**Need Help?**

- 📖 Read this documentation
- 🐛 Report bugs: [GitHub Issues](https://github.com/goldcyper/ns_echo_bot/issues)
- 💬 Ask questions: [GitHub Discussions](https://github.com/goldcyper/ns_echo_bot/discussions)
- ⭐ Star the repo if this helps!

---

## 📝 License

See LICENSE file for details.

---

**Last Updated:** January 5, 2026  
**Version:** 2.0  
**Maintained by:** goldcyper

Made with ❤️ for the NationStates community
