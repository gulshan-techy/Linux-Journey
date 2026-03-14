# Linux Storage Management: Formatting, Mounting, and Persistent Volumes

## 📌 Overview

When you attach a new block storage device (like an AWS EBS volume or a physical hard drive) to a Linux server, the operating system detects the hardware, but you cannot use it immediately to store files. 

This guide covers the complete storage lifecycle across two phases:
* **Phase 1 (Day 21):** Disk Formatting and Temporary Mounting.
* **Phase 2 (Day 22):** Configuring the File System Table (`/etc/fstab`) for Permanent Mounting.

---

## 📅 Phase 1 (Day 21): Disk Formatting & Temporary Mounting

### 📖 The Mounting Lifecycle
To make raw block storage usable, you must follow a strict 3-step lifecycle:
1. **Format:** Create a filesystem on the raw disk (e.g., `ext4`, `xfs`).
2. **Mount Point:** Create an empty directory to act as a gateway.
3. **Mount:** Connect the formatted disk to that directory.

### 🛠️ Practical Lab: The Temporary Mount

**Step 1: Identify the Attached Disk**
First, list all block devices to find the hardware name of the newly attached disk.
```bash
lsblk
```
*(Note: In modern AWS EC2 instances, NVMe drives are typically named `/dev/nvme1n1` or `/dev/nvme2n1`.)*

**Step 2: Format the Raw Disk (`mkfs`)**
If the disk is brand new, it has no filesystem. We must format it to make it ready to store files.
```bash
sudo mkfs.ext4 /dev/nvme1n1
```
*⚠️ **Warning:** Formatting permanently deletes all existing data on that specific disk! Ensure you have selected the correct drive.*

**Step 3: Create a Mount Point**
Create an empty "gateway" directory where you will access the disk's contents.
```bash
sudo mkdir /mnt/data
```

**Step 4: Mount the Disk**
Attach the formatted disk to the empty folder.
```bash
sudo mount /dev/nvme1n1 /mnt/data
```

**Step 5: Verify the Mount**
Check the disk space and mount points to confirm it is successfully attached.
```bash
df -h
```

### 😱 The Reboot Test (Why it's Temporary)
What happens if you save a file (`echo "Hello World" > /mnt/data/hello.txt`) and then reboot the server?

When the server comes back online, running `cat /mnt/data/hello.txt` will return a **"No such file or directory"** error. 

**Why?** Because the standard `mount` command only creates a connection in the active RAM. A reboot clears the RAM, breaking the connection. The `hello.txt` file is still 100% safe on the `/dev/nvme1n1` disk, but you must run the `mount` command again to access it.

### 🔌 Safely Detaching a Disk (`umount`)
If you want to disconnect a disk (perhaps to attach it to a different server or resize it), you should **never** just force-detach it from the AWS Console. You must safely unmount it first to prevent data corruption.
```bash
# Ensure you are NOT currently inside the /mnt/data directory
cd ~

# Safely unmount the disk (Note the spelling: umount, no 'n')
sudo umount /mnt/data
```

---

## 📅 Phase 2 (Day 22): Permanent Mounting and `/etc/fstab`

### 📖 The Problem
As demonstrated in Phase 1, temporary mounts disappear after a reboot. For production servers (like databases or web servers hosting media), storage volumes must automatically reconnect whenever the server starts. 

To make a mount persistent across reboots, we must configure the **File System Table** (`/etc/fstab`).

### 🛠️ Step-by-Step Lab: Making Storage Permanent

**Step 1: Format the Disk (If not already done)**
Ensure the disk has a filesystem.
```bash
sudo mkfs.ext4 /dev/nvme1n1
```

**Step 2: Find the UUID (Industry Best Practice)**
Never use hardware device names like `/dev/nvme1n1` in the `fstab` file. Hardware names can shift if you add or remove disks, which will cause your server to crash during boot. Instead, use the **Universally Unique Identifier (UUID)**.

```bash
sudo blkid /dev/nvme1n1
```
*Output Example:* `UUID="9e5c93b9-79c5-4dc6-845a-feabfd888f4c"`

**Step 3: Create the Mount Point**
```bash
sudo mkdir /my-data
```

**Step 4: Configure `/etc/fstab`**
Open the file system table configuration file using a text editor:
```bash
sudo nano /etc/fstab
```

Add the following line at the very bottom of the file. (Replace the UUID with your actual UUID from Step 2):
```text
UUID=9e5c93b9-79c5-4dc6-845a-feabfd888f4c  /my-data  ext4  defaults  0  2
```

**Understanding the `fstab` Fields:**
| Field | Value | Explanation |
| :--- | :--- | :--- |
| **Device** | `UUID=9e5c...` | The unique ID of the disk. |
| **Mount Point**| `/my-data` | The directory where the disk will be accessible. |
| **File System**| `ext4` | The format type of the disk. |
| **Options** | `defaults` | Standard mounting options (rw, suid, dev, exec, auto, nouser, async). |
| **Dump** | `0` | Used by backup utilities (0 means don't back up). |
| **Pass** | `2` | Order in which `fsck` checks the disk for errors at boot (0=skip, 1=root, 2=others). |

**Step 5: Test the Configuration (`mount -a`)**
*⚠️ CRITICAL STEP:* If you make a typo in `/etc/fstab` and reboot, your server will fail to boot and enter emergency mode. Always test your configuration before rebooting!

```bash
# Mounts all filesystems listed in /etc/fstab
sudo mount -a

# Verify it is mounted successfully
df -h
```
If `sudo mount -a` returns no errors and `df -h` shows your disk attached to `/my-data`, your configuration is perfect. The mount will now survive all future reboots!

---

## 🚀 Conclusion

Mastering the Linux storage lifecycle is a core skill for Cloud and DevOps engineers. 
* Use `mkfs` and `mount` for quick, temporary disk access.
* Always use `blkid` to find the UUID and carefully edit `/etc/fstab` for permanent, production-grade storage persistence.
