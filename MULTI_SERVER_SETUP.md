# 🌐 Multi-Server Setup Guide

<div align="center">

![Multi-Server](https://img.shields.io/badge/Multi--Server-Support-FF6B6B?style=for-the-badge&logo=discord&logoColor=white)
![Multi-Region](https://img.shields.io/badge/Multi--Region-Management-4CAF50?style=for-the-badge&logo=world&logoColor=white)
![Guild-Aware](https://img.shields.io/badge/Guild--Aware-2196F3?style=for-the-badge&logo=settings&logoColor=white)

**Run your bot on unlimited Discord servers with independent region management! 🚀**

[🏠 Back to Main README](../README.md) • [📨 Telegram Targeting](TELEGRAM_TARGETING_GUIDE.md) • [🔄 Migration Guide](MIGRATION_GUIDE.md)

</div>

---

## 🎯 Overview

The Echo Bot now supports **unlimited Discord servers** with **independent NationStates region management**. Each server (guild) can:

<div align="center">

| 🌍 **Independent Regions** | 🔐 **Separate Credentials** | 👥 **Isolated Operations** |
|:--------------------------:|:--------------------------:|:-------------------------:|
| Unique region assignments | Individual NS accounts | Per-server verification |
| Multiple regions per server | Encrypted X-AutoLogin | Guild-specific settings |

</div>

**Perfect for:**
- 🏰 **Regional Organizations** - Multiple servers managing the same region
- 🌐 **Multi-Region Diplomats** - Different servers for different regions
- 🎮 **Game Moderators** - Separate credentials and settings per community

---

## 🗄️ Database Architecture

### 🆕 **New Tables for Multi-Guild Support**

#### `guild_regions` - Server-to-Region Mapping
```sql
CREATE TABLE guild_regions (
    guild_id INTEGER NOT NULL,
    region_id INTEGER NOT NULL,
    PRIMARY KEY (guild_id, region_id),
    FOREIGN KEY (region_id) REFERENCES regions(id)
)
```

**Example Data:**
```
guild_id: 123456789 (Server 1) → region_id: 1 (lazarus)
guild_id: 987654321 (Server 2) → region_id: 1 (lazarus)
guild_id: 111222333 (Server 3) → region_id: 2 (the_east_pacific)
```

#### `guild_ns_credentials` - Encrypted Credentials Storage
```sql
CREATE TABLE guild_ns_credentials (
    guild_id INTEGER PRIMARY KEY,
    general_nation TEXT NOT NULL,
    general_x_autologin TEXT NOT NULL,      -- 🔐 Encrypted with Fernet
    telegram_nation TEXT,
    telegram_x_autologin TEXT,              -- 🔐 Encrypted with Fernet
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

> **🔒 Security:** All X-AutoLogin values are encrypted before database storage!

#### `verified_nations` - Per-Guild Verification
```sql
CREATE TABLE verified_nations (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    guild_id INTEGER NOT NULL,
    discord_user_id TEXT NOT NULL,
    nation_name TEXT NOT NULL,
    verified_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(guild_id, discord_user_id)
)
```

**Example Data:**
```
guild_id: 123456789, discord_user_id: "987654321", nation_name: "spain"
guild_id: 111222333, discord_user_id: "987654321", nation_name: "testlandia"
```

> **💡 Per-Guild Verification:** Same Discord user can verify as different nations across servers!

#### `residents` - Enhanced Resident Tracking
```sql
CREATE TABLE residents (
    nation_id INTEGER PRIMARY KEY,
    wa INTEGER DEFAULT 0,
    region_id INTEGER,
    endorsements INTEGER DEFAULT 0,
    over_cap INTEGER DEFAULT 0,
    reason TEXT,
    citizen INTEGER DEFAULT 0,
    regional_officer INTEGER DEFAULT 0,
    ip_addresses TEXT,                    -- 🔐 Encrypted
    locked INTEGER DEFAULT 0,
    ex_resident INTEGER DEFAULT 0,
    forum TEXT,
    forum_format TEXT,
    discord TEXT,
    verified INTEGER DEFAULT 0,           -- 🔄 Auto-syncs with verified_nations
    banned INTEGER DEFAULT 0,             -- 🚫 Excludes from automatic updates
    FOREIGN KEY (nation_id) REFERENCES nations(id),
    FOREIGN KEY (region_id) REFERENCES regions(id)
)
```

**🚀 Key Features:**
- `verified` column automatically syncs with `verified_nations`
- `banned` column protects residents from automatic updates
- Multi-field search (name, IP, Discord ID, Forum ID) with banned status display
- Encrypted IP addresses for privacy
- Forum integration (XenForo, phpBB, etc.)
- Discord mention support

---

## ⚡ Quick Setup Guide

### 🎛️ **Recommended: Interactive Menu Setup**
```
?menu
```

**Step-by-Step:**
1. **🔧 Setup Bot Permissions** - Add admins, view current permissions
2. **🔑 Setup X-Auto-Login** - Configure NS credentials (DM-based for security)
   - The bot will automatically assign your region when you set up credentials
3. **🌍 Region Management** - Add additional regions if needed (via menu)
4. **👥 Residents Management** - Import data, search residents, manage verification
5. **📊 Endorsement Caps** - Set citizen/non-citizen limits
6. **📨 Auto Telegram** - Configure 4 telegram types with guild-awareness

### ⌨️ **Alternative: Using Commands**

All bot setup is done through the interactive menu system. Type `?menu` and navigate to:

#### 1. 🎯 Configure Credentials (Auto-assigns Region)
**Menu Path:** `?menu` → Setup & Settings → Setup X-Auto-Login

The bot will ask via DM:
- Nation name
- Email
- Password  
- Region

**Your region is automatically assigned when you configure credentials!**

#### 2. 🌍 Add Additional Regions (Optional)
**Menu Path:** `?menu` → Region Management → Enable Region

Enter additional region names if you want to monitor multiple regions.

#### 3. ✅ Verify Configuration
```
?guild_config
```

Shows your current setup including assigned regions, credentials status, and channel configuration.

---

## 🔧 Configuration Details

### Managing Regions

**View Regions:**
```
?guild_config
```
Shows all assigned regions for your server.

**Add More Regions:**
Use `?menu` → Region Management → Enable Region

**Note:** Your primary region is automatically assigned when you set up X-Auto-Login credentials.

### Managing Credentials
```bash
?menu → Residents List                    # Access management menu
?listresident [search]                    # Multi-field search
?resident_info <nation>                   # Detailed info (via DM)
?residents [search]                       # Cross-guild resident search
```

**🔍 Advanced Search Capabilities:**
- Nation name (exact or fuzzy)
- IP address (`192.168.*` wildcards)
- Discord ID (17-19 digits, with/without `<@ID>`)
- Discord username (`@user` format)
- Forum ID (numeric or `forum:123`)

#### 5.1 🤝 Cross-Guild Residents Access
```bash
?menu → Residents List → Cross-Guild Access  # Manage access permissions
```

**🔒 Controlled Data Sharing:**
- **Grant Access:** Admins can authorize other guilds to view their residents data
- **Permission-Based IP Visibility:** IPs only visible to admins from accessing guilds
- **Cross-Guild Search:** `?residents [search]` searches across all authorized guilds
- **Privacy Protection:** Results sent via DM, maintaining confidentiality

**Example:** Guild A grants Guild B access → Users in Guild B can search Guild A's residents via `?residents`, but IP addresses only visible to Guild B admins.

#### 6. ✅ Nation Verification
```bash
?verify <nation_name>        # Start verification (via DM)
?whois @user                 # Check user's verified nation
?whois nation_name           # Check nation ownership
```

**🔒 Secure Verification Process:**
1. `?verify New Rogernomics` (spaces auto-convert to underscores)
2. Bot sends DM with instructions
3. Visit [NS verification page](https://www.nationstates.net/page=verify_login)
4. Login and copy verification code
5. Paste code in DM (2-minute timeout)
6. Bot verifies with NS API and saves

---

## 🎭 Use Case Examples

### 🏰 **Scenario 1: Two Servers, Same Region**

**Server 1: Lazarus Main Government**
```bash
?setup_guild_region lazarus
?setup_guild_credentials
  → General Nation: lazarus_gov
  → Telegram Nation: lazarus_telegram
```

**Server 2: Lazarus Military**
```bash
?setup_guild_region lazarus
?setup_guild_credentials
  → General Nation: lazarus_military
  → Telegram Nation: lazarus_mil_telegram
```

*Both servers manage Lazarus but with separate credentials and operations!*

### 🌐 **Scenario 2: Different Regions**

**Server 3: The East Pacific**
```bash
?setup_guild_region the_east_pacific
?setup_guild_credentials
  → General Nation: tep_delegate
  → Telegram Nation: tep_recruiter
```

*Completely independent TEP management!*

---

## 🛠️ Developer API Reference

### Python Functions for Integration

```python
from db_echo import (
    add_guild_region, remove_guild_region, get_guild_regions,
    set_guild_credentials, get_guild_credentials,
    add_verified_nation, get_verified_nation, remove_verified_nation
)
from utils.nationstates.api import verify_nation

# Region Management
add_guild_region(guild_id, "lazarus")
regions = get_guild_regions(guild_id)  # Returns: [('lazarus', 1), ('balder', 2)]

# Credentials (Auto-encrypted/decrypted)
set_guild_credentials(
    guild_id=123456789,
    general_nation="my_nation",
    general_x_autologin="abc123...",
    telegram_nation="telegram_nation",
    telegram_x_autologin="xyz789..."
)
creds = get_guild_credentials(guild_id)

# Verification Management
add_verified_nation(guild_id=123, discord_user_id=456, nation_name="spain")
nation = get_verified_nation(guild_id=123, discord_user_id=456)  # "spain"
remove_verified_nation(guild_id=123, discord_user_id=456)

# Sync Verification Status
from db_echo import sync_verified_nations_to_residents
stats = sync_verified_nations_to_residents()  # {'verified': 10, 'unverified': 5}

# Update Individual Resident
from db_echo import update_resident_verification
update_resident_verification("spain", discord_id=456, verified=True)
```

---

## 🔄 Migration Guide

### 📈 **For Existing Single-Server Setups**

1. **🚀 Run the Bot** - New tables auto-create
2. **⚙️ Configure Your Server:**
   ```bash
   ?setup_guild_region <your_region>
   ?setup_guild_credentials
   ```
3. **🔧 Update Custom Code** - Use guild-specific functions

### ✅ **Backwards Compatibility**
- `target_regions` table still works
- Bot permissions use `guild_id`
- New guilds start empty

---

## 🛡️ Security Features

<div align="center">

| 🔐 **Encrypted Storage** | 💬 **DM-Only Setup** | 👑 **Admin-Only Access** | 🏰 **Per-Guild Isolation** |
|:------------------------:|:--------------------:|:------------------------:|:-------------------------:|
| Fernet encryption for X-AutoLogin | Private credential entry | Server admin restrictions | Complete data separation |

</div>

---

## 🆘 Troubleshooting

<div align="center">

| Problem | Solution |
|:--------|:---------|
| **🌍 No regions assigned** | Run `?setup_guild_region <region_name>` |
| **🔑 Credentials not configured** | Use `?menu → Setup X-Auto-Login` |
| **📨 Can't send DM** | Enable DMs from server members in Discord settings |
| **❌ Verification failed** | Check nation name conversion, correct login, use code within 2 minutes |
| **📄 WA applicants 400 error** | Bot auto-limits to 15 nations, splits long messages |
| **🔌 Unclosed connector warnings** | Harmless aiohttp cleanup - no action needed |
| **🌐 Multiple regions per guild** | Run `?setup_guild_region` multiple times |

</div>

---

## 🏗️ Architecture Benefits

<div align="center">

| ✅ **Scalable** | ✅ **Isolated** | ✅ **Secure** | ✅ **Flexible** | ✅ **Maintainable** |
|:---------------:|:---------------:|:-------------:|:---------------:|:-------------------:|
| Unlimited servers | Independent operations | Encrypted credentials | Shared or unique regions | Clean database schema |

</div>

---

<div align="center">

**🎉 Scale your NationStates Discord presence across unlimited servers!**

[⬆️ Back to Top](#-multi-server-setup-guide) • [🏠 Main README](../README.md) • [📨 Telegram Targeting](TELEGRAM_TARGETING_GUIDE.md)

</div>
