# Developer Guide - Echo Bot Code Documentation

This guide explains what each major function does in the Echo Bot codebase.

---

## Table of Contents

- [bot.py - Main Bot File](#botpy---main-bot-file)
- [db_echo.py - Database Functions](#db_echopy---database-functions)
- [echo_api.py - Data Processing](#echo_apipy---data-processing)
- [Module Structure](#module-structure)

---

## bot.py - Main Bot File

### Startup and Initialization

#### `check_and_install_dependencies()`
**Purpose:** Checks if all required Python packages are installed  
**When it runs:** Automatically on bot startup  
**What it does:**
- Checks for discord.py, requests, psutil, cryptography, lxml, aiohttp, schedule
- If packages are missing, tries to install them automatically via pip
- Exits the bot if installation fails (requires manual installation)
- Requires bot restart after successful installation

#### `check_and_import_echo_api_data()`
**Purpose:** Imports NationStates data from backup files on first run  
**When it runs:** Once during bot initialization  
**What it does:**
- Checks if `echo_api.db` exists and has data
- If empty, looks for SQL backup files in `echo_api_deploy/` folder
- Imports nation and region data from backup files
- This pre-populates the database so bot has data immediately

### Permission System

#### `allowed_user()`
**Purpose:** Decorator to check if user has basic bot permissions  
**Who can use it:** Users added via `?menu` → Setup Bot Permissions → Add User  
**What commands it protects:** Most regular bot commands (not admin-only)  
**Example:**
```python
@bot.command()
@allowed_user()
async def my_command(ctx):
    # Only users with permission can run this
```

#### `allowed_admin()`
**Purpose:** Decorator to check if user has admin bot permissions  
**Who can use it:** Admins added via `?menu` → Setup Bot Permissions → Add Admin  
**What it gives access to:**
- Bot configuration and setup
- User permission management
- Database operations
- Telegram automation configuration
- Sensitive verification features

#### `user_has_permission(member, permission_type, guild_id)`
**Purpose:** Check if a Discord user has specific permissions  
**Parameters:**
- `member`: Discord member object
- `permission_type`: "user", "admin", or "nation_info"
- `guild_id`: Discord server ID (each server has independent permissions)
**Returns:** True if user has permission, False otherwise

### Core Bot Functions

#### `get_prefix(bot, message)`
**Purpose:** Get the command prefix for the current Discord server  
**What it does:**
- Looks up prefix from database (e.g., '?', '!', etc.)
- Each server can have its own custom prefix
- Defaults to '?' if not configured
**Example:** If prefix is '?', commands are `?help`, `?verify`, etc.

#### `send_discord_message(channel_id, message)`
**Purpose:** Send a message to Discord from non-async code  
**When to use:** Called by modules that don't use async/await (telegram automation)  
**What it does:**
- Takes a regular function call and makes it work with Discord's async API
- Creates appropriate async context to send the message
- Includes rate limiting to prevent Discord API errors
**Example:**
```python
send_discord_message(channel_id=123456789, message="Hello Discord!")
```

### Memory Management

#### `force_memory_cleanup()`
**Purpose:** Force Python to free up unused memory  
**When it's called:** After large operations like processing nation dumps  
**What it does:**
- Runs Python's garbage collector
- Closes unused database connections
- Helps prevent memory issues on low-RAM servers

#### `MemoryOptimizedConnectionPool`
**Purpose:** Manage database connections efficiently  
**Why it exists:** Opening many database connections uses lots of memory  
**What it does:**
- Limits to maximum 2 connections at once
- Reuses connections instead of creating new ones
- Automatically closes idle connections after 30 seconds

#### `MemoryPressureMonitor`
**Purpose:** Watch how much RAM the bot is using  
**What it tracks:**
- Current memory usage in MB
- Warning threshold (default 400 MB)
- Critical threshold (default 600 MB)
**What happens on high memory:**
- Logs warnings
- Triggers automatic memory cleanup
- Can pause certain operations to prevent crashes

### Database Operations

#### `get_db_connection(timeout=120.0)`
**Purpose:** Get a connection to the SQLite database  
**Parameters:**
- `timeout`: How long to wait if database is locked (seconds)
**What it does:**
- Opens connection to `echo.db`
- Retries up to 10 times if database is locked (multiple processes accessing it)
- Returns usable database connection object
**Important:** Always close the connection when done to prevent locks

#### `get_db_transaction(timeout=60.0)`
**Purpose:** Run multiple database operations as a single transaction  
**Why use it:** If one operation fails, all are rolled back (keeps data consistent)  
**Example:**
```python
with get_db_transaction() as (conn, cursor):
    cursor.execute("INSERT INTO nations ...")
    cursor.execute("UPDATE residents ...")
    # Both happen together or neither happens
```

### Bot Event Handlers

#### `on_ready()`
**Purpose:** Runs when bot successfully connects to Discord  
**What it does:**
1. Prints bot connection info (username, server count)
2. Starts background tasks (memory monitor, scheduled dumps)
3. Starts proxy verification server (for citizenship applications)
4. Begins automated telegram scheduler
5. Logs that bot is ready to use

#### `on_command_error(ctx, error)`
**Purpose:** Handles errors when users run commands  
**What it does:**
- Shows friendly error messages for common problems
- "Missing permissions" if user lacks access
- "Command not found" for typos
- "Missing argument" if command needs more info
- Logs detailed errors for debugging

### Key Commands

#### `?menu`
**Purpose:** Opens interactive menu system  
**What it shows:**
- Setup & Settings
- Region Management
- Channel Setup
- Residents Management
- Telegram Automation
- Endorsement Caps
- Citizenship Applications

#### `?guild_config`
**Purpose:** Show current server configuration  
**What it displays:**
- Command prefix
- Configured regions
- Channel IDs (log, WA, updates)
- Credentials status
- Permission settings

#### `?verify <nation_name>`
**Purpose:** Verify you own a NationStates nation  
**How it works:**
1. Bot sends DM with verification code
2. You visit NationStates.net verification page
3. Login and copy the code
4. Paste code in DM (2-minute timeout)
5. Bot confirms and saves to database

#### `?whois <@user or nation_name>`
**Purpose:** Check who owns a nation or what nation a user owns  
**Examples:**
- `?whois @JohnDoe` → Shows which nation JohnDoe verified
- `?whois New_Rogernomics` → Shows which Discord user owns that nation

#### `?newnations <region_name>`
**Purpose:** List new World Assembly nations in a region  
**What it shows:**
- Nations that recently joined WA
- Useful for endorsement campaigns
- Limited to 15 nations per message (prevents Discord errors)

#### `?citizenship`
**Purpose:** Start citizenship application process  
**What happens:**
1. Bot asks questions via DM (nation, reason, etc.)
2. Sends verification link to check for proxies
3. Posts complete application to transcript channel
4. Admins can review and approve/deny

---

## db_echo.py - Database Functions

### Encryption Functions

#### `load_encryption_key()`
**Purpose:** Load or generate encryption key for sensitive data  
**What it does:**
- Checks if `key.key` file exists
- If yes, loads the key
- If no, generates new key and saves it
**Security:** This key encrypts passwords, API keys, IP addresses

#### `encrypt_value(value, fernet)`
**Purpose:** Encrypt a text string  
**Parameters:**
- `value`: The text to encrypt
- `fernet`: The encryption key object
**Returns:** Encrypted string that looks like random characters  
**Example:** Encrypts "mypassword123" to "gAAAAABh..."

#### `decrypt_value(value, fernet)`
**Purpose:** Decrypt an encrypted string back to original text  
**Parameters:**
- `value`: The encrypted string
- `fernet`: The encryption key object
**Returns:** Original plain text  
**Example:** Decrypts "gAAAAABh..." back to "mypassword123"

### Core Database Functions

#### `init_echo_db()`
**Purpose:** Create database tables if they don't exist  
**When it runs:** Automatically on bot startup  
**What tables it creates:**
- `setup` - Bot configuration per server
- `nations` - NationStates nations data
- `regions` - NationStates regions data
- `residents` - Resident tracking and verification
- `verified_nations` - Discord user → nation links
- `citizenship_applications` - Application submissions
- `auto_telegram_settings` - Telegram automation config
- And many more...

#### `get_db_connection(timeout=120.0)`
**Purpose:** Get database connection with retry logic  
**Why retry:** Multiple bot processes might access database simultaneously  
**What it does:**
- Tries to open connection
- If locked, waits and retries (up to 10 attempts)
- Returns working connection or raises error

#### `execute_query(query, params=())`
**Purpose:** Run a database query safely  
**Parameters:**
- `query`: SQL query string (e.g., "SELECT * FROM nations")
- `params`: Values to insert in query (prevents SQL injection)
**Example:**
```python
execute_query("SELECT * FROM nations WHERE name = ?", ("Test_Nation",))
```

### Guild (Server) Management

#### `add_guild_region(guild_id, region_name)`
**Purpose:** Link a Discord server to a NationStates region  
**What it does:**
- Creates entry in `target_regions` table
- Server can now monitor that region
- Multiple servers can monitor same region

#### `get_guild_regions(guild_id)`
**Purpose:** Get list of regions a Discord server is monitoring  
**Returns:** List of (region_id, region_name, enabled) tuples  
**Example:** `[(1, 'lazarus', 1), (2, 'the_pacific', 0)]`

#### `set_guild_credentials(guild_id, general_nation, general_x_autologin, ...)`
**Purpose:** Save NationStates login credentials for a server  
**What it stores (encrypted):**
- General credentials (for RMB posting)
- Telegram credentials (for sending telegrams)
**Security:** All credentials encrypted before storage

#### `get_guild_credentials(guild_id)`
**Purpose:** Load and decrypt NationStates credentials  
**Returns:** Dictionary with nation names and X-AutoLogin tokens  
**Used by:** RMB posting, telegram automation, API calls

### Nation Verification

#### `add_verified_nation(guild_id, discord_user_id, nation_name)`
**Purpose:** Record that a Discord user owns a specific nation  
**What it does:**
- Stores Discord user ID → nation name link
- Per-server (same user can verify different nations on different servers)
- Supports multiple nations per user

#### `get_verified_nation(guild_id, discord_user_id)`
**Purpose:** Get the primary verified nation for a Discord user  
**Returns:** Nation name or None if not verified  
**Example:** Returns "new_rogernomics" for user 123456789

#### `get_verified_user(guild_id, nation_name)`
**Purpose:** Get Discord user who owns a specific nation  
**Returns:** Discord user ID or None  
**Example:** Returns "123456789" for nation "new_rogernomics"

#### `remove_verified_nation(guild_id, discord_user_id)`
**Purpose:** Remove verification link between user and nation  
**When used:** User leaves server or verification needs to be reset

### Resident Management

#### `update_resident_verification(nation_name, discord_user_id, verified=True)`
**Purpose:** Update resident database when user verifies  
**What it does:**
- Sets `verified` flag to True/False
- Links Discord ID to resident record
- Keeps resident data in sync with verification

#### `sync_verified_nations_to_residents()`
**Purpose:** Sync all verifications to resident database  
**When to run:** After importing residents from CSV  
**What it does:**
- Loops through all verified_nations entries
- Updates corresponding resident records
- Ensures data consistency

### Token Management

#### `store_encrypted_bot_token(token)`
**Purpose:** Save Discord bot token encrypted in database  
**Why:** Safer than storing in plain text files  
**What it does:**
- Encrypts the bot token
- Stores in `settings` table
- Can be loaded on bot startup

#### `load_bot_token()`
**Purpose:** Load Discord bot token from database  
**Returns:** Decrypted bot token string  
**Fallback:** If not in database, tries `settings.txt` file and migrates it

---

## echo_api.py - Data Processing

### Main Service Class

#### `EchoDailyDumpsService`
**Purpose:** Background service that downloads NationStates data dumps  
**What it does:**
- Downloads nations.xml.gz daily (all nations data)
- Downloads regions.xml.gz daily (all regions data)
- Processes XML and stores in `echo_api.db`
- Runs on a schedule (daily at 2 AM)

### Database Operations

#### `init_database()`
**Purpose:** Create echo_api.db database schema  
**What tables:**
- `nation` - All NationStates nations
- `region` - All NationStates regions
- `nation_region_history` - Historical movement data
- `cards` - Trading cards data

#### `restore_from_backup_if_exists()`
**Purpose:** Import data from backup files on first run  
**What it looks for:**
- `echo_api_deploy/` folder with SQL backup files
- `echo_api_deployment.db.gz` compressed backup
**Why:** Faster than downloading full dumps (2+ GB) on first run

### Data Processing

#### `download_and_process_nations()`
**Purpose:** Download and process nations dump  
**What it does:**
1. Downloads nations.xml.gz from NationStates
2. Extracts XML data
3. Parses each nation's information
4. Inserts into database in batches (500 at a time)
5. Takes 10-30 minutes depending on internet speed

#### `download_and_process_regions()`
**Purpose:** Download and process regions dump  
**What it does:**
1. Downloads regions.xml.gz from NationStates
2. Extracts XML data
3. Parses each region's information
4. Updates database with region data
5. Faster than nations (only ~20,000 regions vs 200,000+ nations)

#### `extract_nation_fields(nation_elem, nation_id, region_mapping)`
**Purpose:** Parse XML for one nation and extract all data  
**What it extracts:**
- Name, fullname, motto, category
- Population, influence, endorsements
- WA status, region, last activity
- Freedom scores (civil rights, economy, political)
- And 50+ other fields

### Memory Optimization

#### `force_memory_cleanup()`
**Purpose:** Free memory during large data processing  
**When called:** After processing each batch of nations/regions  
**Why needed:** Processing 200,000+ nations uses lots of RAM

#### `XMLProcessingOptimizer`
**Purpose:** Process large XML files without loading entire file into RAM  
**How it works:**
- Reads XML element by element (streaming)
- Processes each element
- Immediately clears it from memory
- Allows processing multi-gigabyte files on low-RAM servers

---

## Module Structure

### modules/auto_telegram.py
**Purpose:** Automated telegram campaigns  
**Main functions:**
- `recruitment_main()` - Welcome new nations
- `restorer_main()` - Target nations that moved regions
- `endorsements_main()` - Request endorsements
- `over_cap_main()` - Ask nations to reduce endorsements

### modules/ns_api_helpers.py
**Purpose:** Helper functions for NationStates API calls  
**Main functions:**
- `fetch_region_wanations_api()` - Get WA nations in region
- `fetch_nation_happenings_endorsements()` - Get endorsement data
- `normalize()` - Convert nation names to standard format

### modules/wa_update.py
**Purpose:** World Assembly monitoring  
**Main functions:**
- `wa_update_main()` - Check for new WA proposals/resolutions
- Process GA (General Assembly) and SC (Security Council)
- Post updates to Discord channels

### handlers/handle_x_auto_login.py
**Purpose:** NationStates credential management  
**What it does:**
- Collects credentials via DM
- Encrypts and stores in database
- Validates credentials work with NationStates API

### handlers/handle_resident.py
**Purpose:** Resident search and management  
**Main functions:**
- Search residents by name, Discord ID, IP
- Display resident information
- Update resident records

### utils/logging_utils.py
**Purpose:** Centralized logging system  
**Functions:**
- `log_info()` - Log informational messages
- `log_error()` - Log errors with traceback
- `log_warning()` - Log warnings
- Saves to `logs/bot_YYYYMMDD.log` files

---

## Common Workflows

### Bot Startup Sequence
1. Check dependencies → install if missing
2. Initialize database → create tables if needed
3. Load bot token → from database or settings.txt
4. Connect to Discord → login with bot token
5. Start background tasks → memory monitor, scheduled dumps
6. Start proxy server → for citizenship applications
7. Bot ready → `on_ready()` event fires

### User Verification Flow
1. User types `?verify Nation_Name`
2. Bot sends DM with instructions
3. User visits NationStates.net verification page
4. User copies verification code
5. User pastes code in DM
6. Bot calls NationStates API to verify code
7. Bot saves Discord user ID → nation name link
8. User can now use verified-only commands

### Citizenship Application Flow
1. User types `?citizenship`
2. Bot starts DM conversation
3. Bot asks Q1: What is your nation name?
4. Bot asks Q2: Why do you want to join?
5. Bot asks Q3: Any additional info?
6. Bot generates verification URL
7. User clicks URL → visits proxy check page
8. Page detects IP and checks if it's a proxy/VPN
9. Results sent back to bot
10. Bot posts complete application to transcript channel
11. Admins review and approve/deny

### Telegram Automation Flow
1. Admin configures telegram type via `?menu`
2. Bot runs scheduled check (every 1-2 hours)
3. Bot queries database for target nations
4. Applies targeting rules (excluded regions, etc.)
5. Bot sends telegrams via NationStates API
6. Marks nations as "telegram sent" to avoid duplicates
7. Respects API rate limits (30 seconds between telegrams)

---

## Tips for Developers

### Adding a New Command
```python
@bot.command(name="mycommand", help="Description of what this does")
@allowed_user()  # or @allowed_admin()
async def my_command(ctx, arg1, arg2):
    """
    Detailed explanation of what this command does.
    
    Args:
        ctx: Discord context (provides server, user, channel info)
        arg1: Description of first argument
        arg2: Description of second argument
    """
    try:
        # Your command logic here
        await ctx.send("Command executed!")
    except Exception as e:
        log_error(f"Error in mycommand: {e}")
        await ctx.send(f"Error: {str(e)}")
```

### Database Best Practices
```python
# Always use context manager for transactions
with get_db_transaction() as (conn, cursor):
    cursor.execute("INSERT INTO ...")
    cursor.execute("UPDATE ...")
    # Auto-commits on success, rolls back on error

# Always use parameterized queries (prevents SQL injection)
cursor.execute("SELECT * FROM nations WHERE name = ?", (nation_name,))

# Close connections when done
conn = get_db_connection()
try:
    # Do database work
finally:
    conn.close()
```

### Memory Management
```python
# For large operations, clean up memory periodically
for batch in process_large_dataset():
    process_batch(batch)
    if batch_count % 10 == 0:
        force_memory_cleanup()
```

---

## Need More Help?

- Check `README.md` for user documentation
- See `TESTING_GUIDE.md` for testing procedures
- Review `MULTI_SERVER_SETUP.md` for multi-server configuration
- Open an issue on GitHub for questions

---

**Last Updated:** December 28, 2025
