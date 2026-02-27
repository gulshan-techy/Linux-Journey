# 🕵️‍♂️ Day 14: Linux Log Management & Troubleshooting (Amazon Linux 2023)

When a system or application fails, logs are the only evidence left behind. Mastering log management is essential for any DevOps Engineer to identify root causes and minimize downtime.

### 📁 Common Log Locations in Amazon Linux 2023 (`/var/log`)
Amazon Linux 2023 has moved towards modern logging systems. Here are the key files:
* **`/var/log/cloud-init.log`**: Extremely important in AWS. It logs the provisioning and initialization steps of the EC2 instance.
* **`/var/log/dnf.log`**: Logs all package installations, updates, and removals (AL2023 uses `dnf` instead of `yum`).
* **`/var/log/secure`**: Logs for all authentication, SSH logins, and security-related events.
* **`/var/log/journal/`**: The directory where `systemd-journald` stores its binary logs.

---

### 📜 Mastering `journalctl` (Systemd Logs)
Because Amazon Linux 2023 heavily relies on `systemd`, `journalctl` is the primary tool for querying logs.

* **`journalctl`** : Displays all system logs.
* **`journalctl -u <service_name>`** : Filters logs for a specific service (e.g., `sshd`, `nginx`).
* **`journalctl -p err`** : Displays only logs with an "Error" priority level. Crucial for fast troubleshooting!
* **`journalctl --since "1 hour ago"`** : Shows logs generated only within the last hour.
* **`journalctl -f`** : Follows the logs in real-time (similar to `tail -f`).

---

### 🛠️ Core Troubleshooting Tools
* **`tail -n 50 /var/log/secure`**
  Quickly view the last 50 lines of the security log to check for failed SSH attempts.
* **`dmesg | tail -n 20`**
  Prints the message buffer of the kernel. Used to detect hardware, driver, or boot issues (like Out Of Memory errors).
* **`grep` with Logs**
  Combining grep with logs to find specific text:
  `sudo grep -i "failed" /var/log/secure`

---
*Tip: If you find an error in the logs, copy-paste it into a search engine—chances are someone has already fixed it!* 🚀
