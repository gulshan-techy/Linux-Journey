# 💾 Day 22: Permanent Mounting and `/etc/fstab`

## 📖 The Problem with Temporary Mounts

As we learned previously, using the standard `mount` command only creates a temporary connection in the system's RAM. 
If you run `sudo mount /dev/nvme1n1 /my-data` and then reboot your server, the `/my-data` folder will be completely empty when you log back in! 

To make a storage mount persistent across reboots, we must configure the Linux **File System Table**, located at `/etc/fstab`.

---

## 🛠️ Step-by-Step Lab: Making Storage Permanent

### Step 1: Format the Disk (If New)
If you are attaching a brand new disk (like a fresh AWS EBS volume), you must format it with a filesystem first.
```bash
sudo mkfs.ext4 /dev/nvme1n1
```

### Step 2: Find the UUID (Best Practice)
Never use hardware device names like `/dev/nvme1n1` or `/dev/xvdf` in the `fstab` file. Hardware names can randomly change if you add or remove disks, which will cause your server to crash on boot. 

Instead, always use the **UUID (Universally Unique Identifier)**.

```bash
sudo blkid /dev/nvme1n1
```
*Output Example:* `/dev/nvme1n1: UUID="9e5c93b9-79c5-4dc6-845a-feabfd888f4c" TYPE="ext4"`
*(Copy just the UUID value without the quotes).*

### Step 3: Create the Mount Point
Create the directory where the disk will be permanently attached.
```bash
sudo mkdir -p /my-data
```

### Step 4: Configure `/etc/fstab`
Open the configuration file using a text editor like `vim` or `nano`:
```bash
sudo vim /etc/fstab
```
Add your disk at the bottom of the file using the standard 6-column format. 

**Understanding the 6 Columns:**

| 1. Device | 2. Mount Point | 3. Filesystem | 4. Options | 5. Dump | 6. Pass |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `UUID=9e5c...` | `/my-data` | `ext4` | `defaults` | `0` | `0` |

**Add this line to your file:**
```text
UUID=9e5c93b9-79c5-4dc6-845a-feabfd888f4c   /my-data   ext4   defaults   0   0
```

### Step 5: The Safety Test (CRITICAL) ⚠️
> **WARNING:** Never reboot your server without testing your `fstab` file first! A syntax error in `/etc/fstab` will cause the Linux boot sequence to fail, throwing the server into Emergency Mode.

Run the mount all command:
```bash
sudo mount -a
```
* **If it outputs nothing:** Your syntax is perfect.
* **If it shows an error:** Fix the `/etc/fstab` file immediately before rebooting!

### Step 6: Verify and Reboot Test
Once the safety test passes, verify the mount, write some test data, and reboot to prove it works.
```bash
# 1. Check if it mounted successfully
df -h

# 2. Write some test data to the disk
echo "Hello World! It is permanent mounting." | sudo tee /my-data/hello.txt

# 3. Reboot the server
sudo reboot
```

### Step 7: Post-Reboot Verification
Once the server is back online, SSH back into it and check your file:
```bash
cat /my-data/hello.txt
```
If you see your text, congratulations! Your mount survived the reboot. 🎉

---

## 🔌 Bonus: Safely Detaching Storage (`umount`)

If you ever need to remove the disk (e.g., to resize an EBS volume in AWS or physically remove a drive), you **must** unmount it first to avoid data corruption.

```bash
# 1. Ensure you are not currently inside the directory
cd ~

# 2. Unmount the disk safely
sudo umount /my-data
```

> **Important Note:** Running `umount` only detaches it temporarily. To permanently remove the disk so the server doesn't look for it on the next boot, you must also delete its corresponding line from `/etc/fstab`!
