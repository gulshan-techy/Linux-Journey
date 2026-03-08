#  Day 16 of Linux : System & Environment Variables in Shell Scripting

In Shell Scripting, variables act as containers for storing data. While you can create your own variables (User-Defined Variables), Linux already maintains a set of built-in variables called **System Variables** (or Environment Variables). 

Understanding these is a core DevOps skill because they make your scripts dynamic, portable, and "self-aware."

---

### 🌍 What are Environment Variables?
Environment variables are dynamic values that affect the processes or programs running on a server. They contain information about the system's behavior, the currently logged-in user, and the working environment.

To see all the environment variables currently active on your Linux system, you can run:
`printenv`
OR
`env`



---

### 🔑 The Most Important System Variables
System variables are conventionally written in **UPPERCASE**. Here are the most critical ones you will use in your DevOps automation scripts:

| Variable | Description | Practical Use Case in Scripting |
| :--- | :--- | :--- |
| `$USER` | The username of the currently logged-in user. | Greeting the user or logging who executed a script. |
| `$HOME` | The absolute path to the user's home directory. | Saving backups dynamically (e.g., `tar -czf $HOME/backup.tar.gz /data`). |
| `$PWD` | Print Working Directory (where you are right now). | Executing files relative to the script's current location. |
| `$HOSTNAME`| The network name of the machine/server. | Identifying which EC2 instance generated an alert or log. |
| `$SHELL` | The path to the current user's default shell. | Verifying if the user is running bash or zsh. |
| `$UID` | The User ID of the current user. | Checking if the user is `root` (Root UID is always `0`). |

---

### 🛣️ The Almighty `$PATH` Variable
`$PATH` is arguably the most important environment variable in Linux. It contains a colon-separated list of directories that the shell searches when you type a command.

If a command or script is not in one of the directories listed in `$PATH`, you have to type its full absolute path (e.g., `/usr/bin/python3` instead of just `python3`).

**To view your path:**
`echo $PATH`

---

### 🔍 Command Substitution (The DevOps Secret Weapon)
Besides predefined variables, you can capture the exact output of *any* Linux command and store it inside a variable. This is called Command Substitution.

**Syntax:** `VARIABLE_NAME=$(command)`

**Examples:**
``` 
# Storing the current date in a variable
CURRENT_TIME=$(date +"%Y-%m-%d %H:%M:%S")
echo "Backup started at: $CURRENT_TIME"

# Storing available free RAM
FREE_RAM=$(free -m | grep Mem | awk '{print $4}')
echo "You have $FREE_RAM MB of RAM available."
