# ⚡ Day 19: Mastering Temporary Aliases in Linux

## 📌 Overview

In Linux, an `alias` acts as a custom shortcut or abbreviation for a longer, more complex command sequence. For System Administrators and DevOps Engineers, mastering aliases is a fundamental skill to save time, reduce repetitive typing, and minimize syntax errors during live operations.

**Temporary Aliases** are strictly session-specific. They exist only in the system's volatile memory (RAM) for the duration of your current terminal window. The moment you log out, close the SSH session, or reboot the server, these aliases are completely erased.

---

## 🛠️ The Syntax of an Alias

Creating a temporary alias is incredibly straightforward. You use the `alias` command followed by the name you want to assign, an equals sign, and the command string.

**Format:**
```bash
alias short_name="actual_long_command"
```

### ⚠️ Important Rules:
* **No Spaces Around Equals:** There must be **NO spaces** before or after the `=` sign. (e.g., `alias name = "command"` will throw a syntax error).
* **Quote Your Commands:** The actual command should always be enclosed in double quotes `""` or single quotes `''`. This ensures that any spaces, flags, or special characters (like pipes `|`) within the command are parsed and executed correctly.

---

## 💻 Practical DevOps Examples

Here are some highly practical, real-world aliases used during operations on an Amazon Linux server.

### 1. The Quick System Update Shortcut
Instead of typing the full package manager update command every single time, map it to a single, memorable word.

```bash
alias update="sudo yum update -y"
```
*Now, simply typing `update` will trigger the full system update automatically, bypassing the confirmation prompt.*

### 2. The Web Server Log Shortcut
When troubleshooting Apache, you frequently check the access or error logs. You can create a shortcut to instantly view the last 5 lines without typing the absolute path.

```bash
alias apache_log="tail -5 /var/log/httpd/access_log"
```

### 3. The Root Disk Space Checker
Aliases can also encapsulate command pipelines (multiple commands combined with a pipe `|`). Here, we filter the `df -h` output to display only the root `/` partition.

```bash
alias diskcheck="df -h | grep '/$'"
```

---

## 🔍 Viewing and Managing Aliases

Creating aliases is only half the battle. You also need to know how to audit and remove them from your active session.

### 1. View All Active Aliases
To see a list of every alias currently active in your session (including default system aliases like `ll='ls -l'`), simply type:
```bash
alias
```

### 2. Remove a Specific Alias (`unalias`)
If you made a typo during creation or no longer need a specific shortcut, use the `unalias` command followed by the custom alias name.
```bash
unalias diskcheck
```
*Verification:* If you try to run `diskcheck` after executing the `unalias` command, the terminal will return a `bash: diskcheck: command not found` error. This confirms the alias was successfully wiped from the session.

### 3. Remove ALL Temporary Aliases at Once
If you want to wipe the slate clean and remove all custom aliases for the current session simultaneously, use the `-a` flag:
```bash
unalias -a
```

---

## 💡 Pro-Tip for Live Troubleshooting

Temporary aliases are perfect for quick scripting, log parsing, or rapid troubleshooting during a live incident. Because they do not modify permanent configuration files, you can experiment freely without the fear of permanently breaking any system configurations or affecting other users on the server!

---

## ⏭️ Coming Up Next

Temporary aliases are great, but what if you want to keep your shortcuts forever? In **Day 20**, we will move beyond temporary session limits and learn how to configure **Permanent Aliases**, ensuring your favorite commands survive reboots and new SSH sessions!
