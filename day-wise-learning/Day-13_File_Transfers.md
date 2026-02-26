# ☁️ Day 13: Linux File Transfer & Downloading Tools

In the DevOps and Cloud world, your code lives on your local machine, but it needs to run on remote servers (like AWS EC2, Azure VMs). Transferring files securely and efficiently across the network is a daily task. 

Here is my complete toolkit for downloading and transferring files in Linux:

---

### 1️⃣ Downloading Files from the Internet
When you need to pull scripts, packages, or compressed files directly to your server.

* **`wget <URL>`**
  The most common non-interactive network downloader. It downloads files directly from the web to your current directory.
  **Example:** `wget https://example.com/software-v1.zip`
  **Pro-Tip:** Use `wget -c <URL>` to resume a broken or interrupted download!

* **`curl -O <URL>`**
  While `curl` is mostly used for API testing (Day 8), the `-O` (capital O) flag tells it to save the output as a file with its original name.
  **Example:** `curl -O https://example.com/script.sh`

---

### 2️⃣ Secure Copy Protocol (`scp`)
`scp` is used to copy files or directories between a local and a remote system, or between two remote systems. It uses SSH for data transfer, meaning your data is fully encrypted. 🔐

* **Local to Remote (Upload):**
  `scp my_app.py username@10.0.0.5:/home/username/`
* **Remote to Local (Download):**
  `scp username@10.0.0.5:/var/log/nginx/error.log ./`
* **Transferring an Entire Directory:**
  Use the `-r` (recursive) flag.
  `scp -r my_project/ ubuntu@aws-ec2-ip:/var/www/html/`
* **Using an SSH Key (AWS EC2 Standard):**
  `scp -i my-key.pem my_app.py ubuntu@aws-ec2-ip:/home/ubuntu/`

---

### 3️⃣ Remote Sync (`rsync`) - The DevOps Standard
`rsync` is a fast and extraordinarily versatile file copying tool. It is vastly superior to `scp` for backups because it uses a "delta-transfer algorithm" (it
