# 📥 Residents Import Guide

<div align="center">

![Import Guide](https://img.shields.io/badge/Import-Guide-4CAF50?style=for-the-badge&logo=google-sheets&logoColor=white)
![Bulk Import](https://img.shields.io/badge/Bulk-Import-FF9800?style=for-the-badge&logo=database&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google-Sheets-34A853?style=for-the-badge&logo=google&logoColor=white)

**Bulk import resident data from Excel, Google Sheets, or CSV into your NationStates Discord bot! 📊**

> **📋 Note:** The `?residents` command now performs cross-guild resident search. Import functionality is accessed through `?menu` → **Residents List** → **"📥 Import Data"**.

[🏠 Back to Main README](../README.md) • [🌐 Multi-Server Setup](MULTI_SERVER_SETUP.md)

</div>

---

## 🚀 Quick Start

<div align="center">

### 🎯 **Three Easy Import Methods**

| 🎛️ **Discord Menu** | 🌐 **Google Sheets URL** | 📄 **CSV File** |
|:--------------------:|:------------------------:|:---------------:|
| **Easiest!** | Direct from Sheets | Traditional file |
| No command line | Always up-to-date | Offline editing |
| Real-time feedback | Team collaboration | Excel compatible |

</div>

### 🎛️ **Option 1: Import via Discord Menu (Recommended!)**

1. **🤖 Use the command:** `?menu`
2. **📋 Navigate to:** **Residents List** category
3. **📥 Click the button:** **"📥 Import Data"**
4. **🔗 Paste your Google Sheets URL** in the modal
5. **🔄 Choose update mode:** Update existing residents? (yes/no)
6. **✅ Done!** Results appear instantly in Discord

> **💡 Note:** The `?residents` command is now used for cross-guild resident search. Import functionality is accessed through the menu system.

### 🌐 **Option 2: Import from Google Sheets URL**

1. **📊 Create your Google Sheet** with proper format
2. **🔓 Share the sheet:** Click "Share" → "Anyone with the link can view"
3. **📋 Copy the Google Sheets URL**
4. **💻 Run the command:**
   ```bash
   python import_residents.py "https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit"
   ```

### 📄 **Option 3: Import from CSV File**

1. **📥 Download template:** `residents_template.csv`
2. **📝 Fill in your data** (see format below)
3. **💾 Export as CSV** from Excel/Google Sheets
4. **💻 Run the command:**
   ```bash
   python import_residents.py your_file.csv
   ```

---

## 📋 File Format

### 📊 **Required Columns**

<div align="center">

| Column | 📝 Description | 🔧 Format | 💡 Example |
|:-------|:---------------|:----------|:-----------|
| **`nation`** | Nation name (required) | Text | `new_rogernomics` or `New Rogernomics` |
| **`region`** | Region name (required) | Text | `lazarus` or `The Pacific` |

</div>

### 🔧 **Optional Columns**

<div align="center">

| Column | 📝 Description | 🔧 Format | 💡 Example |
|:-------|:---------------|:----------|:-----------|
| **`wa`** | WA member status | yes/no, true/false, 1/0 | `yes` or `1` |
| **`endorsements`** | Number of endorsements | Integer | `185` |
| **`over_cap`** | Over endorsement cap | yes/no, true/false, 1/0 | `no` or `0` |
| **`citizen`** | Citizen status | yes/no, true/false, 1/0 | `yes` or `1` |
| **`regional_officer`** | RO status | yes/no, true/false, 1/0 | `yes` or `1` |
| **`reason`** | Reason/note | Text | `Regional Officer Cap Applies` |
| **`discord`** | Discord ID or mention | ID or mention format | `177845312742752256` or `<@177845312742752256>` |
| **`forum`** | Forum user ID | Text or number | `3` or `username123` |
| **`forum_format`** | Forum software | `domain/path,software` | `nslazarus.com/forum,xenforo` |
| **`ip_addresses`** | IP addresses | Comma-separated | `192.168.1.1,10.0.0.1` |
| **`locked`** | Locked status | yes/no, true/false, 1/0 | `no` or `0` |
| **`ex_resident`** | Ex-resident flag | yes/no, true/false, 1/0 | `no` or `0` |

</div>

---

## 📊 Preparing Your Spreadsheet

### 🌐 **Google Sheets Setup (For Discord Import)**

1. **📄 Create a new Google Sheet** or open existing spreadsheet
2. **📋 Set up headers** in the first row (see template below)
3. **✍️ Fill in your data** following the format
4. **🔓 Share the sheet**:
   - Click "Share" button (top right)
   - Click "Change to anyone with the link"
   - Set to "Anyone with the link" → "Viewer"
   - Click "Copy link"
5. **🤖 Use in Discord**:
   - Run `?residents` command
   - Click "📥 Import Data" button
   - Paste the Google Sheets URL
   - Choose update mode (yes/no)
   - Click Submit

<div align="center">

### ✅ **Benefits of Discord Import**
- **🎯 Direct from Discord** - No command line needed
- **📱 Works on any device** - Phone, tablet, desktop
- **⚡ Real-time status** - See results immediately
- **🚨 Error messages** - Clear feedback if something goes wrong
- **🔄 Easy re-import** - Update and re-run anytime

</div>

### 🌐 **Option 1: Google Sheets (Direct URL Import)**

1. **📄 Create a new Google Sheet** or open existing spreadsheet
2. **📋 Set up headers** in the first row (see template)
3. **✍️ Fill in your data** following the format above
4. **🔓 Share the sheet**:
   - Click "Share" button (top right)
   - Click "Change to anyone with the link"
   - Set to "Anyone with the link" → "Viewer"
   - Click "Copy link"
5. **💻 Import directly**:
   ```bash
   python import_residents.py "https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit"
   ```

<div align="center">

### ✅ **Benefits of URL Import**
- **🔄 Always up-to-date** - Import latest data without re-exporting
- **👥 Team collaboration** - Multiple people can update the sheet
- **📚 Version control** - Google Sheets tracks changes automatically
- **🔁 Easy updates** - Re-run with `--update` flag anytime
- **📄 No file management** - No download/upload required
- **📑 Specific sheets** - Import from specific tabs using `#gid=` in URL

</div>

### 📄 **Option 2: CSV Export from Google Sheets/Excel**

1. **📄 Create a new Google Sheet** or open existing spreadsheet
2. **📋 Set up headers** in the first row (see template)
3. **✍️ Fill in your data** following the format above
4. **💾 Export as CSV**:
   - **Google Sheets:** File → Download → Comma Separated Values (.csv)
   - **Excel:** File → Save As → CSV (Comma delimited) (*.csv)
5. **💾 Save the file** to your bot directory

---

## 📋 Example Data Layout

<div align="center">

```
| nation           | region           | ip_addresses    | wa  | endorsements |
|------------------|------------------|----------------|-----|--------------|
| new_rogernomics  | lazarus          | 192.168.1.1    | yes | 185          |
| test_nation      | the_pacific      |                | no  | 0            |
| another_nation   | the_west_pacific | 10.0.0.1       | yes | 42           |
```

**💡 Note:** Only `nation`, `region`, and `ip_addresses` are imported. Other data should be managed through bot commands and menus.

</div>

---

## 🔐 IP Addresses

<div align="center">

| Feature | Details |
|:--------|:--------|
| **📝 Format** | Multiple IPs: `192.168.1.1,10.0.0.1` |
| **🔒 Security** | **Automatically encrypted** when stored |
| **📭 Optional** | Leave blank if no IP data |

</div>

---

## 💻 Running the Import

### 📋 **View Template Format**
```bash
python import_residents.py --template
```

### 🌐 **Import from Google Sheets URL**
```bash
# Direct import from Google Sheets
python import_residents.py "https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit"

# Import and update existing residents
python import_residents.py "https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit" --update

# Import specific sheet (use the gid from the URL)
python import_residents.py "https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit#gid=123456"
```

> **📋 Requirements:** Sheet must be shared as "Anyone with the link can view"

### 📄 **Import from CSV File**

#### 🆕 **Import New Residents Only**
```bash
python import_residents.py residents.csv
```

**Results:**
- ✅ **Add new residents**
- ⏭️ **Skip existing residents**
- 📊 **Show summary of results**

#### 🔄 **Import and Update Existing Residents**
```bash
python import_residents.py residents.csv --update
```

**Results:**
- ✅ **Add new residents**
- 🔄 **Update existing residents** with new data
- 📊 **Show summary of results**

---

## 📊 Import Results

<div align="center">

After running, you'll see a detailed summary:

```
============================================================
📊 IMPORT RESULTS
============================================================
✅ Added: 50
🔄 Updated: 10
⏭️  Skipped: 5
❌ Errors: 2

============================================================
🚨 ERRORS:
============================================================
  Row 15: Missing required field 'nation'
  Row 23: Invalid nation or region name
============================================================
```

</div>

---

## ✅ Data Validation

The import tool automatically:

<div align="center">

| 🔧 **Processing** | 📝 **Details** |
|:------------------|:---------------|
| **🏷️ Normalizes names** | Converts spaces to underscores, lowercases |
| **🆕 Creates missing data** | Nations/regions created if they don't exist |
| **🔒 Encrypts IPs** | IP data encrypted before storage |
| **⚡ Batch commits** | Commits every 100 rows for performance |
| **🎯 Selective updates** | Only nation, region, and IPs imported |

</div>

---

## 🚨 Error Handling

<div align="center">

| 🚨 Error | 🔧 Solution |
|:---------|:------------|
| `Missing required field 'nation'` | Ensure every row has a nation name |
| `Missing required field 'region'` | Ensure every row has a region name |
| `File not found` | Check the CSV file path is correct |
| `Invalid nation or region name` | Remove special characters or empty values |

</div>

---

## 💡 Tips & Best Practices

<div align="center">

1. **🧪 Start small** - Test with 5-10 rows first
2. **📋 Use the template** - Download `residents_template.csv` as starting point
3. **👀 Check your data** - Review in Excel/Sheets before exporting
4. **💾 Backup first** - Run `python create_deployment_backup.py` before large imports
5. **📜 Review logs** - Check `logs/` directory for detailed import logs

</div>

---

## 📝 Complete Examples

### 🎛️ **Example 1: Discord Import (Recommended)**

1. **📊 Create your spreadsheet in Google Sheets**:
   ```
   nation              | region           | ip_addresses
   --------------------|------------------|----------------
   new_rogernomics     | lazarus          | 192.168.1.1
   testlandia          | the_pacific      |
   example_nation      | the_north_pacific | 10.0.0.1
   ```

2. **🔓 Share the sheet**:
   - Click "Share" → "Anyone with the link" → "Viewer"
   - Copy the URL

3. **🤖 Import in Discord**:
   - Type: `?menu`
   - Navigate to: **Residents List** category
   - Click: **"📥 Import Data"** button
   - Paste URL: `https://docs.google.com/spreadsheets/d/1ABC...XYZ/edit`
   - Update existing: `no` (or `yes` to update)
   - Click: **Submit**

4. **📊 View results in Discord**:
   ```
   📊 Import Complete

   ✅ Added: 3
   🔄 Updated: 0
   ⏭️  Skipped: 0
   ❌ Errors: 0
   ```

### 🌐 **Example 2: Google Sheets URL Import**

1. **📊 Create your spreadsheet** (same format as above)
2. **🔓 Share the sheet** and copy URL
3. **💻 Run import directly**:
   ```bash
   python import_residents.py "https://docs.google.com/spreadsheets/d/1ABC...XYZ/edit"
   ```
4. **📊 Review results**:
   ```
   ✅ Downloaded CSV successfully!
   ✅ Added: 3
   ⏭️  Skipped: 0
   ❌ Errors: 0
   ```

### 📄 **Example 3: CSV File Import**

1. **📊 Create your spreadsheet** (same format as above)
2. **💾 Download as CSV** and save as `my_residents.csv`
3. **💻 Run import**:
   ```bash
   python import_residents.py my_residents.csv
   ```
4. **📊 Review results**:
   ```
   ✅ Added: 3
   ⏭️  Skipped: 0
   ❌ Errors: 0
   ```

---

## 🔧 Advanced Usage

### 🌐 **Google Sheets URL Advantages**

<div align="center">

| ✅ **Benefit** | 📝 **Description** |
|:---------------|:-------------------|
| **🔄 Always Up-to-Date** | Import latest data without re-exporting |
| **👥 Team Collaboration** | Multiple people can update the sheet |
| **📚 Version Control** | Google Sheets tracks changes automatically |
| **🔁 Easy Updates** | Re-run with `--update` flag to sync changes |
| **📄 No File Management** | No need to download/upload CSV files |
| **📑 Specific Sheets** | Import from specific tabs using `#gid=` in URL |

</div>

**Example Workflow:**
```bash
# Initial import
python import_residents.py "https://docs.google.com/spreadsheets/d/ABC/edit"

# Later, after team updates the sheet
python import_residents.py "https://docs.google.com/spreadsheets/d/ABC/edit" --update
```

### 🔄 **Update Regions and IPs**

The import system only updates essential identification data:

1. **📄 Create CSV** with `nation`, `region`, and optionally `ip_addresses` columns
2. **💻 Run with `--update` flag**
3. **🔄 Only region and IP addresses** will be updated for existing residents

**Example CSV for bulk updates:**
```csv
nation,region,ip_addresses
new_rogernomics,the_north_pacific,192.168.1.1
test_nation,lazarus,
another_nation,the_west_pacific,10.0.0.1
```

> **💡 Note:** For other resident information (Discord IDs, WA status, etc.), use the bot's commands and menus instead of CSV import.

---

## 🆘 Troubleshooting

### 🌐 **Google Sheets URL Issues**

<div align="center">

| 🚨 Error | 🔧 Solution |
|:---------|:------------|
| **"Access denied" or 403 error** | Make sure sheet is shared with "Anyone with the link can view" |
| **"Google Sheet not found" or 404** | Verify URL is correct and sheet hasn't been deleted |
| **"Invalid Google Sheets URL"** | Copy full URL from Google Sheets address bar |
| **Importing specific sheet/tab** | Add `#gid=123456` to URL for specific tab |

</div>

### 📄 **CSV Encoding Issues**

If you see strange characters:
1. **📊 In Excel:** Save As → CSV UTF-8 (Comma delimited) (.csv)
2. **🌐 In Google Sheets:** File → Download → Comma Separated Values (.csv) (UTF-8 by default)

### 📊 **Excel Date Format Issues**

If Excel converts IDs to dates:
1. **🔧 Format column as "Text"** before entering data
2. **📝 Prefix with apostrophe:** `'177845312742752256`

### 📈 **Large Files**

For files with 1000+ rows:
- **⚡ Import runs batch commits** every 100 rows
- **📊 Monitor console** for progress
- **📜 Check logs** if it takes longer than expected

---

## 🔒 Security Notes

<div align="center">

- **🔐 IP addresses are encrypted** automatically during import
- **💾 Backup before bulk operations** - Always run backups before large imports
- **👀 Review imported data** - Use `?resident <nation_name>` to verify data after import

</div>

---

## 🆘 Support

**Need help with imports?**

1. **🚨 Check error messages** in the console
2. **📜 Review log files** in `logs/` directory
3. **📋 Verify CSV format** matches the template
4. **🧪 Test with small sample** file first

---

<div align="center">

**🎉 Successfully import resident data in bulk!**

[⬆️ Back to Top](#-residents-import-guide) • [🏠 Main README](../README.md)

</div>
