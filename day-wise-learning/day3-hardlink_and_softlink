# 🔗 Day 3: Soft Link vs. Hard Link in Linux

Today, I explored an interesting concepts in the Linux File System: **Links**.
It determines how we create shortcuts and backups for files.

Here is the technical breakdown of what I learned:

## 1. What is an Inode? 🆔
Before understanding links, I learned about **Inodes (Index Nodes)**.
* Every file in Linux has a unique identification number called an Inode.
* It stores metadata (permissions, owner, size) but NOT the filename.
* Command to check Inode: `ls -li`

## 2. Soft Link (Symbolic Link) 📎
Think of this like a **Desktop Shortcut** in Windows.
* It points to the *filename* of the original file.
* If you delete the original file, the Soft Link becomes **broken (useless)**.
* It has a **different Inode number** than the original.

**Command:**
```bash
ln -s <original_file> <soft_link_name>
# Example: ln -s app.log app_shortcut


## 3. Hard Link ⛓️
Think of this as a **Backup Mirror** or a clone.
* It points directly to the **physical data** on the hard disk, not just the name.
* Even if you **delete the original file**, the data remains safe and accessible via the Hard Link.
* It shares the **SAME Inode number** as the original file.

**Command:**
```bash
ln <original_file> <hard_link_name>
# Example: ln abc abc_hard
