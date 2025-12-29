# Memory Monitor Guide

## Overview
The bot now includes a dedicated memory monitoring system that logs memory usage every 2 minutes to help identify memory-intensive operations and optimize them.

## Memory Log Location
Memory logs are automatically created in the `logs/` directory with the naming format:
```
logs/memory_YYYYMMDD.log
```

Example: `logs/memory_20251116.log`

A new log file is created each day automatically.

## Log Format
Each log entry includes:
- **Timestamp**: Date and time of the measurement
- **Marker**: Indicates task start (>>>), task end (<<<), or regular check (---)
- **Task Context**: What operation is currently running or was last active
- **RSS (Resident Set Size)**: Actual physical memory used by the bot process (MB)
- **VMS (Virtual Memory Size)**: Total virtual memory allocated (MB)
- **System Available**: Total free system memory (MB)
- **System Free %**: Percentage of system memory available

### Example Log Entries:
```
2025-11-16 08:30:00 >>> TASK START [Nations Dump Download] | RSS:  245.3 MB | VMS:  512.1 MB | System Available: 1024.5 MB (50.2% free)
2025-11-16 08:32:00 --- [Nations Dump Download] | RSS:  267.8 MB | VMS:  534.2 MB | System Available: 1001.2 MB (49.1% free)
2025-11-16 08:34:15 --- [Nations Dump Processing] | RSS:  598.4 MB | VMS:  812.7 MB | System Available:  725.3 MB (35.6% free)
2025-11-16 08:36:45 <<< TASK END [Nations Dump Complete] | RSS:  289.1 MB | VMS:  545.8 MB | System Available:  989.7 MB (48.6% free)
2025-11-16 08:38:00 --- [Idle] | RSS:  234.2 MB | VMS:  498.3 MB | System Available: 1043.8 MB (51.3% free)
```

## Tracked Operations

The memory monitor automatically tracks these key operations:

### Data Dump Operations
- **Nations Dump Download**: Downloading the 60MB compressed nations XML file
- **Nations Dump Processing**: Parsing and inserting 325k+ nations into database
- **Regions Dump Download**: Downloading the regions XML file
- **Regions Dump Processing**: Parsing and inserting regions into database

### Other Operations
- **Bot Started**: Initial memory state when bot launches
- **Bot Shutting Down**: Final memory state before shutdown
- **Idle**: No active operations (between 2-minute intervals)

## Analyzing Memory Usage

### 1. Identify Memory Spikes
Look for significant RSS increases between measurements:
```
08:30:00 --- [Idle] | RSS:  234.2 MB
08:32:00 --- [Nations Dump Processing] | RSS:  598.4 MB  <- Spike of 364 MB!
```

### 2. Track Operation Memory Footprint
Compare RSS at task start (>>>) vs task end (<<<):
```
08:30:00 >>> TASK START [Nations Dump Download] | RSS:  245.3 MB
08:36:45 <<< TASK END [Nations Dump Complete] | RSS:  289.1 MB
Peak increase: 43.8 MB (acceptable)
```

### 3. Monitor Memory Leaks
If RSS keeps growing over time without returning to baseline after tasks complete, there may be a memory leak:
```
Day 1 - 08:00 [Idle] | RSS:  234 MB
Day 1 - 20:00 [Idle] | RSS:  312 MB  <- Growing
Day 2 - 08:00 [Idle] | RSS:  389 MB  <- Still growing
```

### 4. Check System Memory Pressure
Monitor "System Available" to ensure the hosting provider has enough memory:
```
08:34:15 --- [Nations Dump Processing] | System Available:  725.3 MB (35.6% free)
```
If system memory drops below 500 MB, the hosting provider may kill the process.

## Target Memory Thresholds

Based on hosting provider limits (2GB total):

- **Target RSS (Bot)**: < 1000 MB (1 GB)
- **Warning Level**: 1000-1500 MB
- **Critical Level**: > 1500 MB (risk of being killed by host)

- **System Available**: > 500 MB recommended
- **System Free %**: > 25% recommended

## Using Memory Logs for Optimization

### Step 1: Identify Problematic Operations
Run the bot for 24 hours and review the memory log:
```powershell
Get-Content logs\memory_20251116.log | Select-String "RSS.*[8-9][0-9][0-9]\.[0-9]|RSS.*1[0-9]{3}\.[0-9]"
```
This finds all entries where RSS > 800 MB.

### Step 2: Find Peak Memory Times
```powershell
Get-Content logs\memory_20251116.log | Sort-Object | Select-Object -Last 20
```
Shows the 20 highest memory usage entries.

### Step 3: Correlate with Tasks
Look for the task context in high-memory entries:
```
08:34:15 --- [Nations Dump Processing] | RSS:  987.4 MB  <- This task is the problem!
```

### Step 4: Review Code
Once you identify the problematic task, check its code for:
- Large data structures being built in memory
- Insufficient garbage collection
- Missing streaming/chunking for large files
- Database transaction logs growing too large

### Step 5: Optimize and Test
After making optimizations:
1. Restart the bot
2. Wait for the problematic task to run
3. Compare new memory log entries with old ones
4. Verify RSS stays below 1000 MB

## Integration with Existing Logs

The memory log complements the main bot log (`logs/bot_YYYYMMDD.log`):

**Main Bot Log**: Shows what the bot is doing and any errors
**Memory Log**: Shows memory impact of those operations

Cross-reference them using timestamps to understand both behavior and resource usage.

## Adding Memory Tracking to New Operations

To track memory usage for new operations, use these functions from `memory_monitor.py`:

```python
from memory_monitor import set_active_task, clear_active_task

async def my_new_operation():
    try:
        # Mark operation start
        set_active_task("My New Operation")
        
        # Do work here
        await do_something()
        
        # Mark operation complete
        clear_active_task("My New Operation Complete")
        
    except Exception as e:
        # Mark operation failed
        clear_active_task("My New Operation Error")
        raise
```

## Troubleshooting

### Memory log not being created
- Check if `logs/` directory exists
- Verify bot has write permissions to `logs/` directory
- Check main bot log for errors mentioning "memory monitor"

### Memory log shows only idle entries
- Verify operations are using `set_active_task()` and `clear_active_task()`
- Check if tasks are actually running (check main bot log)

### Memory values seem incorrect
- RSS (Resident Set Size) is the most accurate for actual memory usage
- VMS (Virtual Memory Size) is always higher and includes reserved but unused memory
- Use RSS for optimization decisions

### System memory seems low even when bot RSS is low
- Other processes on the hosting server are using memory
- Check with hosting provider about shared resources
- Consider upgrading hosting plan if consistently low

## Automated Analysis Script

You can create a PowerShell script to analyze memory logs:

```powershell
# memory_analysis.ps1
param([string]$LogFile)

$entries = Get-Content $LogFile

# Find peak memory
$peak = $entries | ForEach-Object {
    if ($_ -match 'RSS:\s+(\d+\.\d+)') {
        [PSCustomObject]@{
            Line = $_
            RSS = [double]$matches[1]
        }
    }
} | Sort-Object RSS -Descending | Select-Object -First 10

Write-Host "`n=== Top 10 Peak Memory Entries ===" -ForegroundColor Yellow
$peak | ForEach-Object { Write-Host $_.Line }

# Find average memory by task
$byTask = $entries | ForEach-Object {
    if ($_ -match '\[(.*?)\].*RSS:\s+(\d+\.\d+)') {
        [PSCustomObject]@{
            Task = $matches[1]
            RSS = [double]$matches[2]
        }
    }
} | Group-Object Task | ForEach-Object {
    [PSCustomObject]@{
        Task = $_.Name
        AvgRSS = ($_.Group | Measure-Object RSS -Average).Average
        MaxRSS = ($_.Group | Measure-Object RSS -Maximum).Maximum
        Count = $_.Count
    }
} | Sort-Object MaxRSS -Descending

Write-Host "`n=== Memory Usage by Task ===" -ForegroundColor Yellow
$byTask | Format-Table -AutoSize
```

Usage:
```powershell
.\memory_analysis.ps1 -LogFile "logs\memory_20251116.log"
```

## Best Practices

1. **Check memory logs daily** during the first week after deployment
2. **Set up alerts** if RSS exceeds 1200 MB
3. **Archive old memory logs** after 30 days to save disk space
4. **Compare before/after** when making optimizations
5. **Monitor trends** over multiple days, not just single spikes
6. **Correlate with disconnect events** in main log to find causes

## Support

If you see unexpected memory behavior:
1. Check the memory log for the time period around the issue
2. Check the main bot log for the same time period
3. Note which task was running when memory spiked
4. Share both log excerpts when requesting support

---

**Memory Monitor Version**: 1.0
**Last Updated**: 2025-11-16
