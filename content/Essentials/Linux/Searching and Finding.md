## awk

> `awk` --> read input line-by-line, split into fields, process them using patterns & actions.

|Symbol|Meaning|
|---|---|
|`$1`|1st field|
|`$2`|2nd field|
|`$0`|Entire line|
|`FS`|Field separator|
|`print`|Output|

```bash
# Print the 1st column
awk '{print $1}' file.txt

# Print lines where 3rd column > 100
awk '$3 > 100' file.txt

# Use comma as separator, print 1st and 3rd columns
awk -F ',' '{print $1, $3}' file.csv

# Sum 2nd column
awk '{sum += $2} END {print sum}' file.txt

# Replace 'foo' with 'bar' in 2nd column
awk '{$2 = "bar"; print}' file.txt
```

## sed

> `sed` --> Edits text line-by-line as it streams through. Used for find & replace, insert, delete, print, multi-edit, etc.

It has **flags** (used for the script behaviour), **commands** (what sed does line-by-line)  and **substitution modifiers** (used for `s///` command).

| Flag        | What it means                 |
| ----------- | ----------------------------- |
| `-e`        | Add multiple editing commands |
| `-f`        | Read commands from a file     |
| `-i`        | Edit the file in-place        |
| `-n`        | Suppress default output       |
| `--version` | Show version info             |
| `--help`    | Show help                     |

| Command | What it does                     |
| ------- | -------------------------------- |
| `s`     | Substitution (find & replace)    |
| `p`     | Print                            |
| `d`     | Delete line                      |
| `a`     | Append text after line           |
| `i`     | Insert text before line          |
| `c`     | Change whole line                |
| `y`     | Transform characters (like `tr`) |
| `=`     | Print line number                |
| `{}`    | Group multiple commands          |

| Modifier | Meaning                                         |
| -------- | ----------------------------------------------- |
| `g`      | Global (all matches per line)                   |
| `1`      | Only the first occurrence (same as default) |
| `2`      | Only the second occurrence                  |
| `3`      | Only the third, etc.                        |
| `p`      | Print the line if a substitution was made       |
| `I`      | Ignore case (GNU sed only)                      |

```bash
# Replace first match per line
sed 's/foo/bar/' file.txt

# Replace all matches per line
sed 's/foo/bar/g' file.txt

# Replace second match per line
sed 's/foo/bar/2' file.txt

# Replace all, ignore case (GNU sed)
sed 's/foo/bar/gI' file.txt

# Replace and print only if changed
sed -n 's/foo/bar/p' file.txt

# Replace in-place
sed -i 's/foo/bar/g' file.txt

# Delete specific line (e.g., 2nd)
sed '2d' file.txt

# Delete lines matching pattern
sed '/pattern/d' file.txt

# Delete a range of lines (2nd to 4th)
sed '2,4d' file.txt

# Print range of lines (1 to 5)
sed -n '1,5p' file.txt

# Print lines matching pattern
sed -n '/foo/p' file.txt

# Print line numbers
sed '=' file.txt

# Insert text BEFORE line 3
sed '3i\
This is a new line' file.txt

# Append text AFTER line 3
sed '3a\
This is appended' file.txt

# Replace entire line 4
sed '4c\
This is the new line' file.txt

# Use multiple commands with -e
sed -e 's/foo/bar/g' -e '/baz/d' file.txt

# Group commands with {}
sed '{s/foo/bar/; /^$/d}' file.txt

# Replace chars a->b and c->d
echo "abc" | sed 'y/ac/bd/'
```


## find

## sort
> `sort` --> sorts lines in text files (alphabetically, numerically, by column, reverse, etc.)

|Flag|Meaning|
|---|---|
|`-n`|Numeric sort (e.g. `2` comes before `10`)|
|`-r`|Reverse order|
|`-k`|Sort by a specific column|
|`-u`|Unique: remove duplicates|
|`-t`|Specify delimiter (default: whitespace)|

```bash
# Sort alphabetically (default)
sort file.txt

# Sort numerically
sort -n numbers.txt

# Sort numerically, reverse order
sort -nr numbers.txt

# Sort by 2nd column (delimiter = comma)
sort -t ',' -k 2 file.csv

# Remove duplicates
sort -u file.txt
```

## grep
> `grep` --> searches input (files or output) line by line for patterns (regex) and prints matching lines. It’s powerful for filtering logs, configs, command output, etc.

```bash
grep "<pattern>" <file> # Find lines with "pattern" in file.
grep "<pattern>" <file1> <file2> ... # Search multiple files.
```

| Flag         | What it does                           | Example                       |
| ------------ | -------------------------------------- | ----------------------------- |
| `-i`         | Ignore upper/lowercase                 | `grep -i "hello" file`        |
| `-v`         | Show lines not matching (invert).      | `grep -v "error" log.txt`     |
| `-r` or `-R` | Recursively search directories.        | `grep -r "TODO" .`            |
| `-n`         | Show line numbers                      | `grep -n "foo" file`          |
| `-c`         | Count matches per file                 | `grep -c "error" log.txt`     |
| `-l`         | Show only filenames with matches       | `grep -l "pattern" *.txt`     |
| `-L`         | Show only filenames without matches    | `grep -L "pattern" *.txt`     |
| `-w`         | Match whole words only                 | `grep -w "cat" file`          |
| `-x`         | Match whole lines only                 | `grep -x "hello world" file`  |
| `-A NUM`     | Show NUM lines After match             | `grep -A 2 "error" log.txt`   |
| `-B NUM`     | Show NUM lines Before match            | `grep -B 2 "error" log.txt`   |
| `-C NUM`     | Show NUM lines Before & After match    | `grep -C 2 "error" log.txt`   |
| `-E`         | Use extended regex (same as `egrep`)   | `grep -E "foo\|bar" file.txt` |
| `-F`         | Interpret pattern literally (no regex) | `grep -F "1.2.3.*" file.txt`  |
It uses "Basic Regex" which means that symbols like `|` , `+`, `?`, `{}` have to be escaped like `\|`, `\+`, `\?`, `\{}`.  
- With -E (Extended Regex), you don’t need the backslashes — so you can write regex like `foo|bar` instead of `foo\|bar`.
- We can also use -F (No Regex), if we want to search for symbols literally.
- `egrep` and `fgrep` are basically the same thing but are **deprecated**.

```bash
# Search dmesg for USB, ignore case
dmesg | grep -i usb

# Check if a service is running
ps aux | grep nginx

# Find TODOs in code, show line numbers and context
grep -rnC 2 "TODO" .

# Show all config lines not commented out
grep -v "^#" config.conf

# Multiple patterns (extended regex)
grep -E "foo|bar|baz" file.txt
```
