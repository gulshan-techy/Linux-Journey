# ⚡ Day 19: Mastering Temporary Aliases in Linux

## 📌 Overview: What is an Alias?

In Linux, an `alias` is a custom shortcut or abbreviation for a longer, more complex command. System Administrators and DevOps Engineers use aliases daily to save time, reduce repetitive typing, avoid typos, and significantly increase productivity on the command line.

**What makes them "Temporary"?**
Temporary aliases are strictly session-specific. This means they only exist in the memory (RAM) of your current terminal session. The moment you log out, close your SSH connection to your server, or reboot the system, these aliases are completely erased.

---

## 🛠️ The Syntax of an Alias

Creating a temporary alias is incredibly simple and can be done directly from the command prompt.

**Standard Format:**
```bash
alias short_name="actual_long_command"
```

**⚠️ Important Rules:**
* **No Spaces Around Equals:** There must be **NO spaces** before or after the `=` sign. (e.g., `alias update = "yum update"` will throw a bash error).
* **Use Quotes:** The actual long command should always be enclosed in double quotes `""` or single quotes `''`. This ensures the shell correctly interprets any spaces, flags, or special characters within the command.

---

## 💻 Practical DevOps Examples (Amazon Linux Server)

Here are some highly practical aliases you might use while managing an Amazon Linux server or preparing your cloud infrastructure:

### 1. The Quick System Update Shortcut
Instead of typing the full package manager update command every single time, map it to a single, easy-to-remember word:
```bash
alias update="sudo yum update -y"
```
*Now, simply typing `update` will trigger a full system update with elevated privileges, automatically answering 'yes' to any prompts.*

### 2. The Web Server Log Shortcut
When troubleshooting a live web server, you often need to check the latest access logs quickly without typing out absolute paths:
```bash
alias apache_log="tail -n 10 /var/log/httpd/access_log"
```
*Typing `apache_log` will instantly print the last 10 lines of the Apache server's access log.*

### 3. The Root Disk Space Checker
You can alias complex command pipelines (commands combined with a pipe `|`). Here, we filter the `df -h` output to show only the root `/` partition to quickly check available storage:
```bash
alias diskcheck="df -h | grep '/$'"
```

---

## 🔍 Viewing and Managing Aliases

Creating aliases is just the first step. You also need to know how to track, manage, and remove them from your active session.

### 1. View all active aliases
To see a complete list of every alias currently active in your session (including default system aliases like `ll='ls -l'`), simply type:
```bash
alias
```

### 2. Remove a specific alias (`unalias`)
If you made a typo while creating an alias or no longer need a specific shortcut, use the `unalias` command followed by the alias name:
```bash
unalias diskcheck
```
*Note: If you try to run `diskcheck` after executing this command, the terminal will return a `bash: diskcheck: command not found` error. This confirms the alias was successfully wiped from the session.*

### 3. Remove ALL temporary aliases at once
If you want to wipe the slate clean and remove all custom aliases for the current session, use the `-a` (all) flag:
```bash
unalias -a
```

---

## 🚀 Bonus: Common Aliases for Cloud Engineers

As you write more Infrastructure as Code, you'll find these shortcuts incredibly helpful:
```bash
# Terraform shortcuts
alias tf="terraform"
alias tfp="terraform plan"
alias tfa="terraform apply -auto-approve"

```

---

## 💡 Pro-Tip

Temporary aliases are perfect for quick scripting, experimenting, or rapid troubleshooting during a live production incident. Because they **do not** modify permanent configuration files (like `~/.bashrc` or `~/.bash_profile`), you can experiment freely without the fear of permanently breaking any user profiles or system configurations! 

*(To make an alias permanent so it survives a reboot, it must be written into the `~/.bashrc` file—which we will cover next!)*
