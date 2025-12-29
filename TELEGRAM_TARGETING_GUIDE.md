# 📨 Telegram Targeting Guide

<div align="center">

![Telegram Automation](https://img.shields.io/badge/Telegram-Automation-0088CC?style=for-the-badge&logo=telegram&logoColor=white)
![Guild-Aware](https://img.shields.io/badge/Guild--Aware-FF6B6B?style=for-the-badge&logo=discord&logoColor=white)
![Smart Targeting](https://img.shields.io/badge/Smart-Targeting-4CAF50?style=for-the-badge&logo=target&logoColor=white)

**Master the art of automated telegram targeting with guild-aware intelligence! 🎯**

[🏠 Back to Main README](../README.md) • [📚 Multi-Server Setup](MULTI_SERVER_SETUP.md) • [🔄 Migration Guide](MIGRATION_GUIDE.md)

</div>

---

## 🎯 Overview

This bot supports **4 types of automated telegrams**, each with different targeting rules and **guild-aware intelligence** that automatically respects your server's region assignments.

<div align="center">

| 📧 **Recruitment** | 🔄 **Restorer** | ⭐ **Endorsements** | ⚠️ **Over-Cap** |
|:------------------:|:---------------:|:------------------:|:---------------:|
| New nations globally | Refounded feeders | WA members in region | Cap violators |
| Guild region excluded | Guild region excluded | Guild region only | Guild region only |

</div>

---

## 📧 Recruitment Telegrams

<div align="center">

### 🌍 **Global Recruitment Network**
**Target:** Newly founded nations (< 30 minutes old)  
**Scope:** Global (all regions except guild's assigned region(s))

![Recruitment](https://img.shields.io/badge/Target-New_Nations-FF9800?style=flat-square&logo=world&logoColor=white)
![Scope](https://img.shields.io/badge/Scope-Global-2196F3?style=flat-square&logo=earth&logoColor=white)

</div>

### 🎯 **Smart Filtering Logic**
- ✅ **Monitors** NationStates happenings feed for founding events
- ❌ **Auto-excludes** guild's assigned region(s) - no manual config needed!
- ❌ **Excludes** additional regions in `excluded_regions` field
- ❌ **Skips** nations with `forbidden` words in name
- ❌ **Ignores** puppet nations (names ending in numbers)

### ⚙️ **Configuration**
```json
{
  "excluded_regions": "the_north_pacific,10000_islands",
  "forbidden": "fascist,reich,nazi"
}
```

> **💡 Pro Tip:** Guild's own region(s) are automatically excluded - even if not in excluded_regions!

**Example:** If your guild manages Lazarus, recruitment telegrams will **never** target Lazarus residents.

---

## 🔄 Restorer Telegrams

<div align="center">

### 🏰 **Feeder Resurrection Service**
**Target:** Refounded nations (nations that ceased to exist and came back)  
**Scope:** **ONLY** Osiris, Lazarus, Balder (feeder regions, excluding guild's region)

![Restorer](https://img.shields.io/badge/Target-Refounded-9C27B0?style=flat-square&logo=recycle&logoColor=white)
![Scope](https://img.shields.io/badge/Scope-Feeders_Only-FF5722?style=flat-square&logo=fire&logoColor=white)

</div>

### 🎯 **Precise Filtering Logic**
- ✅ **Monitors** NationStates happenings feed for refounding events
- ✅ **ONLY** targets nations refounding in `included_regions`
- ❌ **Auto-excludes** guild's assigned region(s)
- ❌ **All other regions** are automatically excluded
- ❌ **Skips** nations with `forbidden` words in name
- ❌ **Ignores** puppet nations (names ending in numbers)

### ⚙️ **Configuration**
```json
{
  "included_regions": "osiris,lazarus,balder",  // Hard-coded!
  "excluded_regions": "additional exclusions within feeders",
  "forbidden": "fascist,reich,nazi"
}
```

> **🚨 Important:** This is the **ONLY** telegram type that uses `included_regions`. Restorer telegrams will **NEVER** be sent outside these 3 regions OR to your guild's region.

**Example:** If your guild manages Lazarus, restorer telegrams target **only** Osiris and Balder.

---

## ⭐ Endorsement Telegrams

<div align="center">

### 🤝 **Delegate Support Network**
**Target:** WA nations in guild's region who haven't endorsed the delegate  
**Scope:** **Guild's assigned region ONLY** (e.g., Lazarus)

![Endorsements](https://img.shields.io/badge/Target-Unendorsed_WA-4CAF50?style=flat-square&logo=star&logoColor=white)
![Scope](https://img.shields.io/badge/Scope-Guild_Region-2196F3?style=flat-square&logo=home&logoColor=white)

</div>

### 🎯 **Intelligent Filtering Logic**
- ✅ **Queries** `residents` table for WA nations in guild's region
- ✅ **Queries** `new_nations` table for WA applicants and newly admitted nations
- ✅ **Uses** `get_target_regions()` - respects guild-region assignments
- ❌ **Skips** nations with `forbidden` words in name
- ❌ **Skips** the delegate themselves
- ❌ **Rate limited** - only contacts each nation once per 24 hours

### 📊 **New Nations Integration**
The bot now tracks WA applicants and newly admitted nations in a dedicated `new_nations` table:

| Status | Description | Targeting Behavior |
|:-------|:------------|:-------------------|
| **Applicant** | Nations that have applied to join WA | Targeted for endorsement reminders |
| **Admitted** | Nations recently admitted to WA | Targeted for endorsement reminders |

**Data Source:** WA happenings API (`fetch_wa_applicants_and_admitted()`)
**Storage:** `new_nations` table with automatic deduplication
**Integration:** Endorsement telegrams now target both established WA members AND new nations

### ⚙️ **Configuration**
- **Automatically** targets region(s) assigned to your guild
- **Uses** verified residents data from your region
- **Forbidden words** filter: `"fascist,reich,nazi"`

> **💡 Note:** This replaces the old "WA Welcome" functionality. Endorsements now handles all WA member engagement within your region.

**Example:** Guild manages Lazarus → endorsement telegrams go **only** to verified Lazarus residents who are in WA but haven't endorsed your delegate, **PLUS** WA applicants and newly admitted nations in Lazarus.

---

## 🆕 New Nations Tracking

<div align="center">

### 📊 **WA Membership Change Monitoring**
**Tracks:** WA applicants and newly admitted nations  
**Scope:** Guild's assigned region(s)  
**Storage:** Dedicated `new_nations` table

![New Nations](https://img.shields.io/badge/New-Nations-00BCD4?style=flat-square&logo=database&logoColor=white)
![WA Tracking](https://img.shields.io/badge/WA-Tracking-2196F3?style=flat-square&logo=world&logoColor=white)

</div>

### 🎯 **How It Works**
- ✅ **Monitors** WA happenings API for membership changes
- ✅ **Stores** applicants and admitted nations in `new_nations` table
- ✅ **Integrates** with endorsement telegrams automatically
- ✅ **Deduplicates** to prevent spam (24-hour cooldown)
- ✅ **Guild-aware** - only tracks regions assigned to your guild

### 📋 **Database Schema**
```sql
CREATE TABLE new_nations (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nation_name TEXT NOT NULL,
    region_name TEXT NOT NULL,
    status TEXT NOT NULL CHECK (status IN ('applicant', 'admitted')),
    timestamp TEXT DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(nation_name, status)
);
```

### 🔄 **Automatic Integration**
New nations are automatically included in endorsement telegram targeting:

| Status | Reason Code | Behavior |
|:-------|:------------|:---------|
| **Applicant** | `new_applicant` | Targeted for endorsement reminders |
| **Admitted** | `new_admitted` | Targeted for endorsement reminders |

> **💡 Pro Tip:** This ensures new WA members get immediate endorsement guidance, improving regional influence!

---

## ⚠️ Over-Cap Telegrams

<div align="center">

### 📊 **Endorsement Cap Enforcement**
**Target:** WA nations exceeding endorsement caps  
**Scope:** **Guild's assigned region ONLY** (e.g., Lazarus)

![Over-Cap](https://img.shields.io/badge/Target-Cap_Violators-FF5722?style=flat-square&logo=warning&logoColor=white)
![Scope](https://img.shields.io/badge/Scope-Guild_Region-2196F3?style=flat-square&logo=home&logoColor=white)

</div>

### 🎯 **Cap Enforcement Logic**
- ✅ **Queries** `residents` table for WA nations marked as `over_cap = 1`
- ✅ **Uses** `get_target_regions()` - respects guild-region assignments
- ❌ **Skips** nations with `forbidden` words in name
- ❌ **Skips** the delegate themselves
- ❌ **Rate limited** - only contacts each nation once per 24 hours

### ⚙️ **Configuration**
- **Automatically** targets region(s) assigned to your guild
- **Requires** endorsement cap limits configured via `?menu`
- **Forbidden words** filter: `"fascist,reich,nazi"`

**Example:** Guild manages Lazarus with 300 endorsement cap → over-cap telegrams go **only** to Lazarus residents exceeding 300 endorsements.

---

## 🎯 **Targeting Summary Matrix**

<div align="center">

| Telegram Type | 🌍 Scope | 🏰 Region Source | 🤖 Guild-Aware | 🎯 Target Criteria |
|:--------------|:--------:|:----------------:|:--------------:|:------------------:|
| **📧 Recruitment** | Global | All regions except guild's & excluded | ✅ Yes | New nations (<30min) |
| **🔄 Restorer** | Feeders Only | Osiris/Lazarus/Balder (minus guild's) | ✅ Yes | Refounded nations |
| **⭐ Endorsements** | Guild Region | Guild assignments | ✅ Yes | Unendorsed WA members + WA applicants/admitted |
| **⚠️ Over-Cap** | Guild Region | Guild assignments | ✅ Yes | Cap violators |

</div>

---

## 🔧 **How Guild-Region Assignment Works**

### 🏗️ **Setup Process**

1. **🎯 Assign Region to Guild:**
   ```
   ?setup_guild_region lazarus
   ```
   *Stores in `guild_regions` table*

2. **✅ Enable Region Monitoring:**
   ```
   ?menu → Setup & Settings → Enable Region
   ```
   *Sets `enabled = 1` in `target_regions`*

3. **🚀 Automatic Targeting:**
   - Endorsements and Over-Cap telegrams use `get_target_regions()`
   - **Guild-aware**: Queries `guild_regions` JOIN `target_regions`
   - **Returns only** regions assigned to your guild AND enabled

---

## 📝 **Configuration Tips**

### 🌐 **For Multi-Guild Bots**
- ✅ Each guild can have its own region(s)
- ✅ Recruitment automatically excludes each guild's assigned region(s)
- ✅ Restorer automatically excludes each guild's assigned region(s)
- ✅ Endorsements and Over-Cap respect guild boundaries

### 🎯 **For Single-Region Focus**
- ✅ Guild's region automatically excluded from recruitment and restorer
- ✅ Add additional regions to `excluded_regions` if needed
- ✅ Restorer targets only the other 2 feeders (not your guild's feeder)
- ✅ Endorsements and Over-Cap automatically target your region

### 🔒 **Security Features**
- 🛡️ `forbidden` words filter inappropriate nation names
- 📋 Default list: `"fascist,reich,nazi"`
- ⚙️ Customize per telegram type in the menu

---

## ⚡ **Quick Reference Guide**

<div align="center">

### 🎯 **Send Telegrams ONLY to Your Region (Lazarus)**
- ✅ **Endorsements** - Already configured (guild-aware)
- ✅ **Over-Cap** - Already configured (guild-aware)
- ✅ **Recruitment** - Automatically excludes Lazarus (guild-aware)
- ✅ **Restorer** - Automatically excludes Lazarus, targets Osiris & Balder (guild-aware)

### 🧪 **Test Your Configuration**
```bash
?telegram_task_status
?run_telegrams
```

</div>

---

<div align="center">

**🎉 Master automated telegram targeting with guild-aware intelligence!**

[⬆️ Back to Top](#-telegram-targeting-guide) • [🏠 Main README](../README.md) • [📚 Multi-Server Setup](MULTI_SERVER_SETUP.md)

</div>
