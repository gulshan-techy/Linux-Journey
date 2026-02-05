🔍 Module 1: Viewing & Analyzing File Content
Commands to read large files and count data without opening editors.
less <filename> (Scrollable View)
Opens large files page-by-page. Allows scrolling up and down.
Example: less /var/log/messages (Press q to exit).
more <filename> (Page View)
Old method to view files. Only allows scrolling forward.
Example: more huge_data.txt
head <filename> (Top Lines)
Displays the first 10 lines of a file by default.
Example: head file.txt (Shows first 10 lines).
Example: head -n 5 file.txt (Shows only first 5 lines).
tail <filename> (Bottom Lines)
Displays the last 10 lines. Very useful for checking latest logs.
Example: tail error.log
Example: tail -f error.log (Live monitoring of new logs).
wc <filename> (Word Count)
Counts lines, words, and characters in a file.
Example: wc -l file.txt (Counts only number of lines).
🕵️‍♂️ Module 2: Advanced Searching
Commands to find files inside folders or text inside files.
grep <text> <filename> (Global Regular Expression Print)
Searches for a specific word or pattern inside a file.
Example: grep "Error" app.log (Shows lines containing "Error").
Example: grep -i "error" app.log (Case insensitive search).
Example: grep -r "failed" /var/log/ (Recursive search inside folder).
find <path> <criteria> (Find Files)
Searches for files based on name, size, or time.
Example (By Name): find /home -name "*.txt" (Finds all text files).
Example (By Size): find / -size +100M (Finds files larger than 100MB).
Example (By Time): find . -mmin -15 (Finds files modified in last 15 mins).
Example (Empty): find . -empty (Finds empty files or folders).
⚙️ Module 3: System Hardware & Monitoring
Commands to check CPU, RAM, and Storage details.
lscpu (CPU Details)
Displays information about the CPU architecture (Cores, Threads, Model).
Example: lscpu
free -h (Memory Usage)
Shows total, used, and free RAM in human-readable format (GB/MB).
Example: free -h
lsblk (List Block Devices)
Lists all storage devices (Hard disks) and their partitions.
Example: lsblk
history
Shows a list of all previously executed commands.
Example: history
Example: !105 (Executes command number 105 from history).
