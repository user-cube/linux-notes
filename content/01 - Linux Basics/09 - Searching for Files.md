---
title: "09 - Searching for Files"
description: "Linux tools for locating files and searching file contents — locate, find, and grep."
tags:
  - linux
  - filesystem
  - grep
  - basics
---

## `locate`

Searches a pre-built database of indexed file paths. Extremely fast because it does not scan the filesystem in real time.

```bash
locate notes.txt
```

> [!warning] Database may be stale
> Newly created or moved files will not appear until the database is updated:
> ```bash
> sudo updatedb
> ```
> On most systems, `updatedb` runs automatically as a daily cron job.

---

## `find`

Searches the filesystem in real time. Always returns up-to-date results at the cost of being slower than `locate`.

```bash
find /home -name notes.txt              # search by name
find /home -iname notes.txt             # case-insensitive name match
find / -type f -name "*.log"            # only regular files
find / -type d -name config             # only directories
find /var/log -mtime -7                 # modified in the last 7 days
find /tmp -size +10M                    # files larger than 10 MB
find /etc -user root                    # files owned by root
find /home -perm 777                    # files with exact permissions
```

### Execute a command on each result

```bash
find /tmp -name "*.log" -exec rm {} \;        # delete each matched file
find /home -name "*.txt" -exec chmod 644 {} \;
```

---

## `grep`

Searches for text patterns **inside files**.

```bash
grep "error" logfile.txt               # basic search
grep -i "error" logfile.txt            # case-insensitive
grep -n "error" logfile.txt            # show line numbers
grep -r "database" /etc                # recursive search through a directory
grep -v "^#" config.conf               # inverted match — exclude comment lines
grep -E "error|warning" logfile.txt    # extended regex — match either pattern
```

### Display context around matches

```bash
grep -A 3 "error" logfile.txt    # 3 lines after the match
grep -B 3 "error" logfile.txt    # 3 lines before the match
grep -C 3 "error" logfile.txt    # 3 lines before and after (context)
```

### Count and list matches

```bash
grep -c "error" logfile.txt      # count matching lines
grep -l "error" /var/log/*.log   # list files that contain the pattern
```

> [!tip] Combining find and grep
> ```bash
> find /etc -name "*.conf" -exec grep -l "timeout" {} \;
> ```
> Lists all `.conf` files under `/etc` that contain the word `timeout`.
