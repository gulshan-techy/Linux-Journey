# Day 12 of Linux 

## The Pipe (|)
The pipe operator takes the standard output of the command on the left and passes it as standard input to the command on the right.
Example: cat file.txt | grep "Error"

## grep (Global Regular Expression Print)
Used to search for specific text patterns within a file or output.

## grep -i: Ignore case sensitivity.

## grep -v: Invert match (show lines that do NOT match).

## awk (Text Processing)
A versatile tool used for manipulating data and generating reports in a column-based format.
Example: awk -F "," '{print $2}' file.csv (Prints the second column of a CSV file).

## sed (Stream Editor)
Used for parsing and transforming text. Most commonly used for "Find and Replace".
Example: sed 's/old/new/g' file.txt (Replaces all occurrences of 'old' with 'new').

## head & tail
Used to view the beginning or end of a file.
Note: tail -f is essential for monitoring live log files in production.
