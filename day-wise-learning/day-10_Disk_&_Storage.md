# 📂 Linux Day 10: Disk and Storage Management

## In a server environment, running out of disk space ("No space left on device") can crash applications and databases. Understanding how to monitor storage, find large files, and mount new drives is a critical skill for managing Linux infrastructure.

## Here is the breakdown of the most important storage management commands:

## ***df -Th (Disk Free)***
Displays the total, used, and available space on all mounted file systems in a human-readable format (GB/MB).
Note: The -T flag also shows the file system type (like ext4 or xfs).

## ***du -sh * (Disk Usage)***
Calculates and displays the exact size of specific directories or files in the current path.
Example: Running sudo du -sh * | sort -h in /var/log will sort and show you exactly which log files are consuming the most space.

## ***lsblk (List Block Devices)***
Displays a tree structure of all attached hard drives, partitions, and their mount points.
Example: Shows devices like sda (standard drives) or nvme0n1 (AWS EC2 volumes).

## ***fdisk -l (Format Disk)***
Lists all the partition tables for the attached disks. This is primarily used to view or manipulate disk partition tables.
Note: This command requires administrative privileges (sudo fdisk -l).

## ***`mount <device> <directory>`***
In Linux, newly attached storage is not automatically accessible. You must "mount" the block device to a specific directory to use it.
Example: sudo mount /dev/sdb1 /mnt/data (Attaches the partition sdb1 to the /mnt/data folder).

## ***`umount <directory>`***
Safely detaches a mounted file system from the directory tree so the drive can be removed or modified.
Example: sudo umount /mnt/data

## ***/etc/fstab (File System Table)***
The critical configuration file that stores information about disk drives and their mount points.
Note: Entries added here ensure that your drives automatically mount every time the Linux server reboots.
