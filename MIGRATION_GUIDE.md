# 🔄 Migration Guide: Single-Guild to Multi-Guild Support

<div align="center">

![Migration](https://img.shields.io/badge/Migration-Guide-FF9800?style=for-the-badge&logo=git&logoColor=white)
![Single→Multi](https://img.shields.io/badge/Single--Guild→Multi--Guild-2196F3?style=for-the-badge&logo=arrow-right&logoColor=white)
![Zero Downtime](https://img.shields.io/badge/Zero-Downtime-4CAF50?style=for-the-badge&logo=clock&logoColor=white)

**Seamlessly transition from single-server to multi-server NationStates management! 🚀**

[🏠 Back to Main README](../README.md) • [🌐 Multi-Server Setup](MULTI_SERVER_SETUP.md) • [📨 Telegram Targeting](TELEGRAM_TARGETING_GUIDE.md)

</div>

---

## 🎯 Overview

This guide helps you **transition an existing single-guild NS Echo Bot installation** to the new **multi-guild architecture** with zero downtime and full backwards compatibility.

<div align="center">

| 🔄 **Zero Downtime Migration** | 🔒 **Data Preservation** | 🔧 **Easy Rollback** |
|:------------------------------:|:-----------------------:|:--------------------:|
| Existing bot keeps running | All data stays intact | Simple revert if needed |
| Automatic schema updates | Credentials preserved | No data loss |

</div>

---

## 📋 Prerequisites

<div align="center">

| ✅ **Requirements** | 🔧 **Access Needed** | 💾 **Backup Recommended** |
|:--------------------:|:--------------------:|:------------------------:|
| Bot running with data | Discord server admin | Database backup before migration |
| Python 3.11+ | Database access | Configuration files |

</div>

---

## ⚡ Migration Steps

### 1. 🗄️ **Database Schema Update**

**🎉 Good News:** The new schema is **automatically created** when you run the bot!

**New Tables Added:**
- `guild_regions` - Maps guilds to regions
- `guild_ns_credentials` - Stores guild-specific credentials
- `verified_nations` - Per-guild verification tracking

> **💡 No Manual SQL Required** - Schema updates handled by `db_echo.py` initialization!

### 2. 🔐 **Migrate Existing Credentials**

#### 🎛️ **Option A: Menu System (Recommended)**
```bash
?menu
```

**Step-by-Step:**
1. **🔧 Setup & Settings** → **Setup X-Auto-Login**
2. **🤖 Choose General** → Follow DM prompts:
   - Nation name
   - Email
   - Password
   - Region
3. **🔄 Bot automatically** retrieves and encrypts X-AutoLogin
4. **📨 Repeat for Telegram** credentials if needed

#### 🛠️ **Option B: Manual SQL Migration (Advanced)**
*For existing `x_auto_login` table data:*

```sql
-- Replace 123456789012345678 with your actual guild_id
-- (Get from Discord: right-click server → Copy ID with Developer Mode)

-- Migrate general credentials
INSERT INTO guild_ns_credentials (guild_id, general_nation, general_x_autologin)
SELECT
    123456789012345678, -- Your guild_id
    n.name,
    x.x_autologin
FROM x_auto_login x
JOIN nations n ON x.nation_id = n.id
WHERE x.enabled = 1 AND x.purpose = 'general'
LIMIT 1;

-- Migrate telegram credentials (if configured)
UPDATE guild_ns_credentials
SET
    telegram_nation = (SELECT n.name FROM x_auto_login x JOIN nations n ON x.nation_id = n.id WHERE x.enabled = 1 AND x.purpose = 'telegram' LIMIT 1),
    telegram_x_autologin = (SELECT x.x_autologin FROM x_auto_login x WHERE x.enabled = 1 AND x.purpose = 'telegram' LIMIT 1)
WHERE guild_id = 123456789012345678; -- Your guild_id
```

### 3. 🌍 **Assign Regions to Guild**

> **💡 Note:** If you used the menu's Setup X-Auto-Login, your region was added automatically!

#### 🔍 **Find Your Target Regions**
```sql
SELECT r.id, r.name
FROM target_regions tr
JOIN regions r ON tr.region_id = r.id
WHERE tr.enabled = 1;
```

#### 🎯 **Option A: Setup Command**
```bash
?setup_guild_region Lazarus
?setup_guild_region The_East_Pacific
```

#### 🛠️ **Option B: Manual SQL**
```sql
-- Replace with your guild_id and region names
INSERT INTO guild_regions (guild_id, region_id)
SELECT
    123456789012345678, -- Your guild_id
    r.id
FROM regions r
WHERE LOWER(r.name) IN ('lazarus', 'the_east_pacific');
```

### 4. ✅ **Verify Migration**

#### 📊 **Check Guild Configuration**
```bash
?guild_config
```

**Should show:**
- ✅ General credentials configured
- ✅ Telegram credentials configured (if applicable)
- 📋 List of assigned regions

#### 🌍 **Check Guild Regions**
```bash
?guild_regions
```

**Should list all regions assigned to your guild.**

#### 🧪 **Test Commands**
```bash
?newnations
?telegram_status
?process_regional_data
```

**All should work with guild-specific data!**

---

## 🌐 Multi-Server Expansion

### 🆕 **Adding Additional Servers**

#### 🏰 **Same Region, Different Credentials**
**Scenario:** Two servers both managing Lazarus with different nations

**Server 1 Setup:**
1. 🤖 **Invite bot** to Server 1
2. 👑 **Add admins:** `?menu` → Setup Bot Permissions
3. 🔑 **Configure:** `?menu` → Setup X-Auto-Login → General (Nation A, Lazarus)

**Server 2 Setup:**
1. 🤖 **Invite bot** to Server 2
2. 👑 **Add admins:** `?menu` → Setup Bot Permissions
3. 🔑 **Configure:** `?menu` → Setup X-Auto-Login → General (Nation B, Lazarus)

#### 🌍 **Different Regions**
**Scenario:** Server 3 managing The East Pacific

**Server 3 Setup:**
1. 🤖 **Invite bot** to Server 3
2. 👑 **Add admins:** `?menu` → Setup Bot Permissions
3. 🔑 **Configure:** `?menu` → Setup X-Auto-Login → General (Nation C, The_East_Pacific)

---

## 🔄 Backward Compatibility

### ✅ **What Still Works**
- **💾 All existing data** (nations, regions, residents) preserved
- **🔑 Old `x_auto_login` table** remains functional (backup)
- **🌍 `target_regions` table** still tracks enabled regions globally

### ⚠️ **What Requires Guild Context**
- ❌ **DM commands** without guild context fail gracefully
- ❌ **Background tasks** now iterate over all guilds
- ⚠️ **Some modules** still need refactoring (see [MULTI_GUILD_PROGRESS.md](MULTI_GUILD_PROGRESS.md))

---

## 🆘 Troubleshooting

<div align="center">

| Problem | Solution |
|:--------|:---------|
| **🔑 No credentials configured** | Run `?setup_guild_credentials` |
| **🌍 No enabled regions assigned** | Run `?setup_guild_region <region_name>` |
| **🏠 Command only works in server** | Run in Discord server channel, not DM |
| **⚙️ Background tasks not running** | Check `?guild_config` and `?guild_regions`, review logs |
| **🔐 Credentials not decrypting** | Re-run `?setup_guild_credentials` to re-encrypt |

</div>

---

## 🔙 Rollback Plan

### 🛡️ **If You Need to Revert**

#### ✅ **Keep Using Old Credentials**
The old `x_auto_login` table is **not removed** - some functions may still reference it.

#### ⚠️ **Remove Guild-Specific Tables (Not Recommended)**
```sql
DROP TABLE IF EXISTS guild_regions;
DROP TABLE IF EXISTS guild_ns_credentials;
```

> **🚨 Warning:** This will lose all guild-specific configuration!

---

## ⚡ Performance Considerations

### 🗂️ **Database Indexes**
**Automatically created:**
- `guild_regions(guild_id)`
- `guild_regions(region_id)`

### 🚀 **Query Optimization**
- Guild-aware queries use indexed lookups
- Background tasks iterate guilds sequentially (not parallel) to avoid rate limits

---

## 🔒 Security Notes

### 🛡️ **Credential Storage**
- **🔐 Fernet symmetric encryption** for all credentials
- **🔑 Encryption key** in environment/configuration
- **🚫 Never log** decrypted credentials

### 👑 **Access Control**
- **🔒 Guild admins only** can configure credentials
- **🏠 Credentials only accessible** within specific guild
- **💬 DM-based setup** prevents channel leakage

---

## 🎯 Next Steps

<div align="center">

1. 📖 **Review** [MULTI_GUILD_PROGRESS.md](MULTI_GUILD_PROGRESS.md) for implementation status
2. 🧪 **Test all commands** with guild context
3. 📊 **Monitor background tasks** for multi-guild processing
4. 🌐 **Consider additional servers** if managing multiple regions

</div>

---

## 🆘 Support

**Need help with migration?**

1. 📋 **Check logs** in `logs/` directory for errors
2. 🔍 **Verify schema:** `?check_schema`
3. 📊 **Review config:** `?guild_config`
4. 🌍 **Check regions:** `?guild_regions`

---

<div align="center">

**🎉 Successfully migrated to multi-guild support!**

[⬆️ Back to Top](#-migration-guide-single-guild-to-multi-guild-support) • [🏠 Main README](../README.md) • [🌐 Multi-Server Setup](MULTI_SERVER_SETUP.md)

</div>
