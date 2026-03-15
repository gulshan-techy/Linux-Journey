# 🌐 Day 23: Configuring Network File System (NFS) on AWS EC2

## 📌 Overview: What is NFS?

**Network File System (NFS)** is a distributed file system protocol originally developed by Sun Microsystems. It allows a user on a client computer to access files over a computer network much like local storage is accessed. 

In a modern AWS Cloud environment, if you have an Auto Scaling Group of web servers that all need synchronized access to the same uploaded media files (images, videos, documents), you use NFS. 

*💡 **Cloud Native Note:** AWS Elastic File System (EFS) is essentially a fully managed, highly available, and scalable NFS service provided by AWS! Understanding manual NFS setup is crucial for understanding how EFS works under the hood.*

---

## 🏗️ AWS Architecture Prerequisites (Critical)

Before running any Linux commands, your AWS infrastructure must be properly configured. Most NFS setups fail at the networking layer, not the OS layer.

1. **Network Placement:** Both EC2 instances (the NFS Server and the NFS Client) should ideally be in the **same VPC and Subnet** to ensure smooth, secure internal IP communication.
2. **Security Groups (Firewall):** You **MUST** allow NFS traffic in the Security Group attached to the NFS Server so the Client can reach it.
   * **Inbound Rule:** Allow **TCP/UDP Port 2049 (NFS)**.
   * **Source:** Restrict this to the Client's Private IP (e.g., `172.31.67.202/32`) or the VPC CIDR block for security.

---

## 🖥️ Part 1: Configuring the NFS Server (Provider)

**Server Internal IP:** `172.31.73.55`

### Step 1: Install Required Packages
The `nfs-utils` package provides the NFS server and related tools.
```bash
# Check if it is already installed
yum list installed | grep nfs-utils

# If not installed, install it
sudo yum install nfs-utils -y
```

### Step 2: Create the Shared Directory and Dummy Data
Create the directory you want to share over the network and populate it with some test files.
```bash
sudo mkdir -p /shared_folder
cd /shared_folder

# Create test data
sudo bash -c 'echo "Hello world! from NFS server" > hello.txt'
sudo touch aa bb cc
```

### Step 3: Configure the Export List
The `/etc/exports` file dictates exactly which clients are allowed to access the shared folder and what permissions they have.
```bash
sudo vi /etc/exports
```
Add the following line to grant the specific Client IP Read/Write (`rw`) access:
```text
/shared_folder  172.31.67.202(rw,sync,no_root_squash)
```
*(Note: `sync` ensures changes are written to disk before replying to the client, and `no_root_squash` allows the client root user to maintain root privileges on the shared folder).*

### Step 4: Apply Exports and Start Services
Reload the export table and start the necessary daemons.
```bash
# Export the directories listed in /etc/exports
sudo exportfs -arv

# Start and enable the NFS and RPC Bind services
sudo systemctl start rpcbind
sudo systemctl start nfs-server

# Ensure they start automatically on reboot
sudo systemctl enable rpcbind
sudo systemctl enable nfs-server
```

---

## 💻 Part 2: Configuring the NFS Client (Consumer)

**Client Internal IP:** `172.31.67.202`

### Step 1: Install Packages
The client also needs the `nfs-utils` package to understand the NFS protocol and use the `mount.nfs` helper program.
```bash
sudo yum install nfs-utils -y
```
*(Note: The client does not necessarily need to run the `nfs-server` daemon, but it does need the utilities).*

### Step 2: Discover Shared Folders
You can query the NFS Server to see what folders it is currently exposing to the network using the `showmount` command.
```bash
showmount -e 172.31.73.55
```
**Expected Output:**
```text
Export list for 172.31.73.55:
/shared_folder 172.31.67.202
```

### Step 3: Mount the Network Drive
Create a local empty directory, which will act as the mount point. Then, mount the Server's exported folder to this local directory.
```bash
# Create the local mount point
mkdir -p ~/nfs_client

# Mount the remote NFS share
sudo mount -t nfs 172.31.73.55:/shared_folder ~/nfs_client
```

### Step 4: Verify the Bidirectional Sync!
Navigate into your local mount point and check the contents.
```bash
cd ~/nfs_client
ls
# Expected Output: aa bb cc hello.txt

cat hello.txt
# Expected Output: Hello world! from NFS server
```

### 🔄 Real-Time Sharing Test
Because we configured `(rw)` (Read/Write) in the `/etc/exports` file on the server, you can test the bidirectional sync.

1. Create a new file on the **Client** side:
   ```bash
   touch ~/nfs_client/client_test.txt
   ```
2. Check the **Server** side `/shared_folder`:
   ```bash
   ls /shared_folder
   # You will instantly see 'client_test.txt' appear!
   ```
They are now perfectly synchronized.

---

## 🚀 Conclusion

By successfully configuring an NFS Server and Client, you have established a centralized file-sharing mechanism. This is a foundational concept for managing stateful data across multiple stateless EC2 instances and serves as the perfect stepping stone before transitioning to managed services like AWS EFS.
