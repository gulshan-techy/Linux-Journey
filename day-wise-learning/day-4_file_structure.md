📂 Linux Day 4: Linux File System Hierarchy (FHS)
Unlike Windows, Linux does not have C: or D: drives. Everything starts from the Root (/). Here is the breakdown of the most important directories:

/ (Root Directory) The top-level directory in the Linux file system hierarchy. It is the parent of all other directories. Note: Only the root user has write permissions here.

/root (Root Home) The home directory specifically for the Superuser (Root). Difference: / is the system root, while /root is the personal folder for the admin.

/home (User Home) Contains personal directories for all local users. Example: If you create a user named "virat", his files will be stored in /home/virat.

/boot (Boot Loader) Contains the static files required to boot the system, including the Linux Kernel (vmlinuz) and GRUB configuration. Example: If this folder is deleted, the OS will not start.

/bin (User Binaries) Contains essential command binaries (executable programs) that can be used by all users (both root and standard users). Example: Common commands like ls, cp, cat, mkdir are stored here.

/sbin (System Binaries) Contains system administration binaries that are intended to be run only by the Superuser (Root). Example: Admin commands like reboot, fdisk, ifconfig, iptables.

/etc (Etcetera / Configuration) Stores all system-wide configuration files and startup scripts. It is the "Control Panel" of Linux. Example: /etc/passwd (User details), /etc/group (Group details), /etc/ssh/sshd_config (SSH settings).

/var (Variable Data) Contains files that are expected to grow in size over time (dynamic data). Example: System logs (/var/log), emails, and print queues.

/tmp (Temporary) Stores temporary files created by system and users. Note: Content inside this directory is usually deleted automatically when the system reboots.

/dev (Devices) Contains special files that represent hardware components. In Linux, "Everything is a file," including hardware. Example: /dev/sda (Hard Disk), /dev/tty (Terminal).

/usr (User System Resources) Contains read-only user data, including installed applications, libraries, and documentation. Example: When you install software like Python or Apache, it usually goes here.

/lib (Libraries) Contains essential shared libraries needed by the binaries in /bin and /sbin. Example: Similar to .dll files in Windows.

/opt (Optional) Used for installing optional or third-party add-on software that is not part of the default Linux installation. Example: Large applications like Oracle Database or Chrome.

/mnt (Mount) A directory used for temporarily mounting external filesystems manually. Example: Mounting a USB stick or an extra hard drive manually.

/media (Removable Media) A mount point for removable media devices that are automatically mounted by the OS. Example: CD-ROMs, USB flash drives (when auto-detected).

/proc (Process Information) A virtual file system that contains information about running processes and the kernel. It does not exist on the hard disk; it exists in RAM. Example: /proc/cpuinfo (CPU details), /proc/meminfo (RAM usage).

/srv (Service Data) Contains site-specific data served by this system. Example: Files for a Web Server or FTP server.
