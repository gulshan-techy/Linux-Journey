# 🌐 Day 8: Linux Networking & Troubleshooting

In Cloud and DevOps, if you can't connect to a server, you can't deploy your application. Understanding how data travels, how domains resolve, and which ports are open is a mandatory skill for any DevOps Engineer.

Here is my complete toolkit for Linux network monitoring and troubleshooting:

---

### 1️⃣ Interface & IP Details (The Basics)
The first step in networking is knowing your own identity on the network.

* **`ip addr show`** (or **`ifconfig`**)
  Displays detailed information about all network interfaces attached to the system.
  **Use Case:** Finding your Private IP address (e.g., `192.168.x.x`) and Interface Name (e.g., `eth0`).

---

### 2️⃣ Connectivity Check (The Heart of Networking)
Verifying if your system can talk to the internet or a specific remote server.

* **`ping -c 4 google.com`**
  Sends ICMP ECHO_REQUEST packets to network hosts. 
  **Note:** The `-c 4` flag limits the test to exactly 4 packets, keeping the terminal output clean and concise.

---

### 3️⃣ DNS Troubleshooting (Name Resolution)
DevOps engineers frequently face DNS issues. These tools prove if your system can successfully translate domain names into IP addresses.

* **`nslookup github.com`**
  Queries the Domain Name System (DNS) to find the underlying IP address mapped to a hostname.
* **`dig linkedin.com`**
  A more flexible and detailed tool for interrogating DNS name servers.

---

### 4️⃣ Path Tracking (Advanced Touch)
When a connection is slow or dropping, you need to see exactly where the delay is happening.

* **`tracepath -n google.com`**
  Traces the route taken by data packets to reach a network host, showing all the "hops" (routers) along the way. The `-n` flag displays IP addresses directly instead of resolving hostnames, making it faster.

---

### 5️⃣ Port & Service Monitoring (Professional Touch)
This is crucial for security and application health. It shows which "doors" (ports) are open on your server.

* **`sudo ss -tulpn`** (or **`netstat -tulpn`**)
  Displays socket statistics. It is the modern replacement for netstat.
  **Flags breakdown:**
  - **`-t`**: TCP connections
  - **`-u`**: UDP connections
  - **`-l`**: Listening ports only
  - **`-p`**: Show Process ID (PID) - *Requires `sudo`*
  - **`-n`**: Numeric output (Don't resolve names)
  **Use Case:** Verifying if SSH (Port 22) or HTTP (Port 80) are actively listening.

---

### 6️⃣ Web Requests & APIs (Transfer Tools)
Testing if a web application or API is successfully responding without needing a web browser.

* **`curl -I https://www.google.com`**
  Fetches the HTTP headers of a website. 
  **Use Case:** Looking for the `HTTP/1.1 200 OK` status code, which proves the web server is healthy and responding correctly.

---
*Networking is 90% of DevOps troubleshooting. If you can't ping it, you can't deploy it!* 🚀
