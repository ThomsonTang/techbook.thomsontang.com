
# Unix for Beginners

## files and common commands

- `ls -t` causes the files to be listed in “time” order, last changed, most recent first.
- `wc` counts the lines, words and characters in one or more files.
- `grep` searches files for lines that match a pattern.
	- `grep pattern files`
	- `grep -v pattern files`: look for lines that **don’t** match the pattern.
- `sort` sorts its input into alphabetical order line by line.
	- `sort -r`: reverse normal order
	- `sort -n`: sort in numeric order
	- `sort -nr`: sort in reverse numeric order
	- `sort -f`: fold upper and lower case together
	- `sort +n`: sort starting at `n+1`-st field