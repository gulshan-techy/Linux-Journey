# 🚀 Day 9: Linux Process Management & System Monitoring

Knowing how to monitor system health and safely manage or kill rogue applications is what separates a beginner from a true DevOps Engineer / System Admin. Here is the complete toolkit for managing processes and monitoring system resources in Linux.

---

### 1️⃣ System Health & Overview
These commands give you a quick high-level summary of how the server is performing.

* **`uptime`**
  Checks how long the system has been running and displays the average CPU load (1-min, 5-min, and 15-min averages).
* **`free -h`**
  Displays total, used, and free Memory (RAM) and Swap space in a human-readable format (GB/MB).
* **`vmstat` & `mpstat`**
  Advanced monitoring tools. `vmstat` reports virtual memory statistics, while `mpstat` provides detailed CPU utilization reports.

---

### 2️⃣ Real-Time Monitoring
For live, dynamic tracking of what is consuming your server's resources.

* **`top`**
  The classic real-time task manager in Linux. It shows live CPU, RAM usage, and a dynamic list of running processes.
* **`htop`**
  A beautifully colored, interactive, and advanced version of `top`. It allows you to scroll vertically/horizontally and kill processes directly (e.g., using `F9`). *(Note: May need to be installed via `apt` or `yum`)*.

---

### 3️⃣ Process Sniffing (Finding the Culprit)
When you need to hunt down a specific process that is running in the background.

* **`ps`**
  Shows a basic snapshot of processes running in your current terminal session.
* **`ps -e`**
  Lists absolutely every process currently running on the system.
* **`ps aux | grep ssh`**
  Filters the massive process list to find only specific services. In this case, it pipes the output of `ps aux` into `grep` to find processes related to `ssh/sshd`.
* **`pstree`**
  Displays processes in a visual tree format, showing parent-child process relationships clearly.

---

### 4️⃣ Job Control (Background vs Foreground)
How to keep your terminal free while long tasks run.

* **`sleep 60 &`**
  Adding `&` at the end of any command forces it to run in the background. This keeps your current terminal prompt free for other work.
* **`jobs`**
  Lists all the background jobs that are currently running or stopped in your specific terminal session.

---

### 5️⃣ Terminating Rogue Processes
Taking control and stopping applications that are stuck or using too much memory.

* **`kill <PID>`**
  Sends a graceful termination signal (SIGTERM) to a specific Process ID. It asks the process to save its data and exit.
* **`killall <name>`**
  Forcefully kills all instances of a process by its application name. 
  **Example:** `killall firefox` will terminate every running Firefox process at once.

---
*If your server is slow, don't just reboot it. Monitor it, find the process, and handle it gracefully! 🛡️*
