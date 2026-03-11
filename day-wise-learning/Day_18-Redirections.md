# 🔀 Day 18: Output Redirection in Linux (`>` and `>>`)

## 📌 Overview

In Linux, when you execute a command, the result is usually printed directly onto your terminal screen. This default behavior is known as **Standard Output** or `stdout`. 

However, in DevOps, system administration, and automation, we rarely want to just *look* at the output on a screen. We frequently need to save this output to a file to create logs, generate reports, or dynamically update configurations.

This is achieved using **Redirection Operators**.



---

## 1️⃣ The Overwrite Operator (`>`)

The single greater-than sign (`>`) is used to redirect the output of a command into a file. It is a **destructive** operation.

**Important Behaviors:**
* 🆕 **Creates new files:** If the target file **does not exist**, Linux will create it automatically.
* ⚠️ **Overwrites existing files:** If the file **already exists**, it will completely remove the old content and overwrite it with the new output. Always use this with caution!

### Practical Examples

**1. Saving command output to a text file:**
```bash
# Saves the list of files and directories into 'directory_list.txt'
ls -l > directory_list.txt
```

**2. Overwriting a configuration file:**
```bash
# Completely replaces the contents of config.env with this single line
echo "app_mode=production" > config.env
```

**3. Truncating (clearing) a file:**
A common DevOps trick to clear the contents of a log file without actually deleting the file itself.
```bash
# Makes 'my_log.txt' completely empty (0 bytes)
> my_log.txt
```

---

## 2️⃣ The Append Operator (`>>`)

The double greater-than sign (`>>`) also redirects output to a file, but it is entirely **non-destructive**.

**Important Behaviors:**
Instead of overwriting the file, it **appends** (adds) the new content to the very bottom of the file, preserving all the older data. This is absolutely crucial for updating ongoing log files or safely adding new lines to a configuration script.

### Practical Examples

**1. Adding to a configuration file safely:**
```bash
# Adds a new variable to the bottom without touching the old data
echo "app_version=2.1.0" >> config.env
```

**2. Tracking events over time:**
```bash
# Appends the current date and time to a server reboot tracker
date >> server_reboots.log
```

**3. Appending command execution results:**
```bash
# Adds the current disk usage statistics to a continuous history file
df -h >> disk_usage_history.txt
```

---

## 📊 Summary Comparison: `>` vs `>>`

| Operator | Name | Behavior | Risk Level | Common Use Case |
| :---: | :--- | :--- | :---: | :--- |
| `>` | Overwrite | Replaces all existing content. Creates file if missing. | 🔴 High | Initializing config files, clearing logs, capturing one-time state. |
| `>>` | Append | Adds to the end of existing content. Creates file if missing. | 🟢 Low | Writing continuous log files, adding history, updating lists. |

---

## 💡 DevOps Real-World Scenario

Imagine you are writing a shell script (e.g., a `cron` job) that runs every night at 2:00 AM to check if a database backup was successful. 

You wouldn't want to use the `>` operator, because that would overwrite yesterday's log, leaving you with only the data from the single most recent day. Instead, you use the append (`>>`) operator to keep a continuous, historical audit trail:

```bash
#!/bin/bash

# ... [backup logic happens here] ...

# Append the success message along with the exact timestamp to the log file
echo "Backup completed successfully on $(date)" >> /var/log/daily_backup.log
```
By using `>>`, your `/var/log/daily_backup.log` will steadily grow day by day, providing a complete history of your backup operations.
