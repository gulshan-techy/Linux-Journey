# 🛡️ Day 7: Linux File Permissions

In Linux, security is baked into the file system. Every file and directory has strict rules about who can read, modify, or execute it. Understanding these permissions is crucial for solving the dreaded "Permission Denied" errors and securing your servers.

Here is a deep dive into Linux Access Control:

---

### 1️⃣ The Anatomy of Permissions (`ls -l`)
When you run `ls -l`, you see a 10-character string (e.g., `-rwxr-xr--`). Here is how to break it down:

* **Character 1 (File Type):** * `-` = Regular File
  * `d` = Directory
  * `l` = Symbolic Link
* **Characters 2-4 (User/Owner):** What the creator of the file can do.
* **Characters 5-7 (Group):** What users in the file's group can do.
* **Characters 8-10 (Others):** What everyone else on the system can do.

**The rwx Values (Octal System):**
* **`r` (Read) = 4** (Can view the file contents / List directory)
* **`w` (Write) = 2** (Can edit the file / Add/Delete files in a directory)
* **`x` (Execute) = 1** (Can run the script / Enter the directory using `cd`)
* **`-` (Denied) = 0**

---

### 2️⃣ `chmod` (Change Mode) - Modifying Permissions
You can change permissions using two methods: Numeric (Absolute) or Symbolic.

**A. Numeric Mode (The DevOps Way)**
Calculated by adding the rwx values together (4+2+1).
* **`chmod 777 file.txt`** 🚩 (Danger: rwx for everyone. Never use in production!)
* **`chmod 755 script.sh`** ✅ (Standard for scripts: Owner gets 7 [rwx], Group & Others get 5 [rx]).
* **`chmod 644 config.yml`** 📄 (Standard for files: Owner gets 6 [rw], Group & Others get 4 [r]).
* **`chmod 600 private.key`** 🔐 (Highly Secure: Only the owner can read/write. Required for SSH keys).

**B. Symbolic Mode (Quick Adjustments)**
Uses letters: `u` (user), `g` (group), `o` (others), `a` (all).
* **`chmod u+x script.sh`** (Adds execute permission for the user/owner).
* **`chmod g-w file.txt`** (Removes write permission from the group).
* **`chmod o=r file.txt`** (Forces others to only have read access).

---

### 3️⃣ Ownership Commands (`chown` & `chgrp`)
Permissions don't matter if the wrong person owns the file.

* **`chown <user> <file>`**
  Changes the owner of the file.
  **Example:** `sudo chown root secret.txt`
* **`chown <user>:<group> <file>`**
  Changes both the owner and the group at the same time.
  **Example:** `sudo chown virat:devops app_config.json`
* **`chown -R <user>:<group> <directory>`**
  **[Advanced]** The `-R` (Recursive) flag changes the ownership of the directory and *everything* inside it. Crucial for fixing web server folder permissions!
* **`chgrp <group> <file>`**
  Changes only the group ownership of a file.

---

### 4️⃣ Pro-Tips: Directory vs. File Permissions
* **Execute (`x`) on a File:** Means you can run it as a program or script.
* **Execute (`x`) on a Directory:** Means you can `cd` into it. If a user doesn't have `x` permission on a folder, they cannot access any files inside it, even if the files themselves have `chmod 777`!

---

### 5️⃣ `umask` (Default Permissions)
* **`umask`**
  Displays or sets the default permissions assigned to newly created files and directories. (Usually set to `022`, meaning new files get `644` and directories get `755`).

---
*Security Rule #1: Principle of Least Privilege. Only give users the exact permissions they need to do their job, nothing more!* 🛑
