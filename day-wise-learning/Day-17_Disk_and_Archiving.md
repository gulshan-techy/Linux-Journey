# 💾 Day 17: Disk Management & Archiving in Linux

Managing storage is a critical day-to-day task for System Administrators and DevOps Engineers. Running out of disk space can bring down databases and web servers instantly. 

Here is the complete toolkit for monitoring storage and creating compressed backups.

---

### 🔍 1. Monitoring Disk Space

#### `df` (Disk Free)
Displays the amount of available disk space for file systems on which the invoking user has appropriate read access.
* **`df -h`** : The `-h` flag makes the output "Human Readable" (prints in MBs and GBs instead of exact byte counts).
* **`df -T`** : Prints the file system type (e.g., ext4, xfs) alongside the usage.

#### `du` (Disk Usage)
Estimates file and directory space usage. Used to find out which specific folder is consuming the most space.
* **`du -sh /var/log`** : 
  * `-s` (Summarize): Shows only the total size of the specified directory, not every individual file inside it.
  * `-h` (Human Readable): Shows sizes in KB, MB, or GB.
* **`du -h --max-depth=1 /var`** : Lists the size of all top-level folders inside `/var`. Great for hunting down large directories!

---

### 📦 2. Archiving and Compression (`tar`)
In the Windows world, `.zip` is common. In Linux, we use `tar` (Tape Archive) to combine multiple files into one, and `gzip` to compress that archive. Files ending in `.tar.gz` or `.tgz` are often called "Tarballs".

#### The Essential `tar` Flags:
* **`-c`** : **C**reate a new archive.
* **`-x`** : e**X**tract an existing archive.
* **`-z`** : Compress the archive using g**Z**ip.
* **`-v`** : **V**erbose. Shows the files being processed on the terminal.
* **`-f`** : Specifies the **F**ilename of the archive. (Always put `-f` at the end of your flags!)

#### 🛠️ Practical Backup Commands
**1. Creating a Compressed Backup:**
Compress the entire `/etc` directory and save it as `etc_backup.tar.gz` in your home folder.
```
sudo tar -czvf ~/etc_backup.tar.gz /etc
```
2. Extracting an Archive:
Extract the contents of a tarball into your current directory.

```
tar -xzvf my_backup.tar.gz
```
3. Extracting to a Specific Directory:
Use the -C (capital C) flag to define the destination path.

```
tar -xzvf my_backup.tar.gz -C /target/directory/
```
4. Viewing Contents without Extracting:
Want to see what's inside the archive before unzipping it? Use the -t flag.

```
tar -tzvf my_backup.tar.gz
```
Pro-Tip: Always check your disk space with df -h before extracting a massive .tar.gz file to ensure your server has enough room for the uncompressed data! 🚀
