# ♾️ Day 20: Permanent Aliases using the `.bashrc` File

## 📖 Overview: The Problem with Temporary Aliases

Temporary aliases (created directly in the terminal using the `alias` command) are stored in the system's volatile memory (RAM). The moment you close your SSH session, log out, or the server reboots, they are permanently erased. 

To create shortcuts that last forever—surviving reboots and session drops—we must define them inside the user's persistent environment configuration file.

---

## ⚙️ Understanding `~/.bashrc`

Every user in a Linux system has a hidden configuration file in their home directory named `.bashrc` (Bash Run Commands). 

Whenever you open a new terminal session or log into the server, the Bash shell reads this file from top to bottom and applies the configurations. By appending our custom aliases to this file, we ensure they are automatically loaded into memory every single time we log in.

---

## 🛠️ Step-by-Step Practical Guide

### Step 1: Open the configuration file
Use a command-line text editor like `vim` or `nano` to edit the file located in your home directory (`~`).
```bash
vim ~/.bashrc
```

### Step 2: Add your custom Aliases
1. Press `i` to enter **Insert Mode** in Vim.
2. Scroll to the absolute bottom of the file.
3. Add your shortcuts. Here is a highly useful daily DevOps toolkit:

```bash
# ==========================================
# My Custom Permanent DevOps Aliases
# ==========================================
alias mk='mkdir'
alias diskcheck='df -h | grep "/$"'
alias pingg='ping [www.google.com](https://www.google.com)'
alias ports='netstat -tulnp'
alias myip='curl ifconfig.me'
```
4. Save and exit the file (Press `Esc`, type `:wq`, and hit `Enter`).

### Step 3: Reload the Configuration (`source`)
Linux only reads the `.bashrc` file during the initial login sequence. To apply your new aliases immediately *without* dropping your current SSH connection, use the `source` command:
```bash
source ~/.bashrc
```

---

## 🧪 Testing the New Toolkit

Now, these short commands will execute complex tasks instantly across your server:

* **`myip`** ➔ Reaches out to the internet and returns your EC2 instance's exact Public IP address.
* **`ports`** ➔ Shows all active listening services, their states, and their Process IDs (PIDs).
* **`diskcheck`** ➔ Filters the file system list to show only the main root directory (`/`) capacity, giving you a quick health check on disk space.
* **`pingg`** ➔ Instantly checks internet connectivity by pinging Google.

---

## 🗑️ Managing Permanent Aliases

### Deleting an Alias Forever
To completely remove an alias so it never loads again:
1. Open the file: `vim ~/.bashrc`
2. Delete the specific `alias ...` line.
3. Save and exit.
4. Reload the profile: `source ~/.bashrc`

### Temporarily Ignoring a Permanent Alias
If you want to remove an alias *only* for the current session without deleting it from your configuration file, use the `unalias` command:
```bash
unalias diskcheck
```
*If you run `diskcheck` after this, the terminal will say "command not found". However, the next time you log in or run `source ~/.bashrc`, the alias will be back!*

---

## 🛡️ Best Practices for DevOps
* **Always comment your aliases:** Add a comment (`#`) above your custom blocks in `.bashrc` so you remember what they do months later.
* **Avoid overriding core system commands:** Do not alias core commands to do destructive things. For example, aliasing `ls` to `ls -la` is helpful, but modifying how `rm` behaves without caution can be dangerous.
