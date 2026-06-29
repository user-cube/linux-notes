---
title: "05 - File Types"
description: "Linux file types — regular files, directories, character/block devices, links, sockets, and named pipes."
tags:
  - linux
  - filesystem
  - basics
---

One of the core Linux principles is:

> **Everything is a file.**

Almost every system object is represented as a file — directories, hard disks, terminals, network sockets, devices, and pipes.

Linux classifies files into three broad categories: **Regular Files**, **Directories**, and **Special Files**.

---

## Identifying File Types

### `ls -l`

The first character of the `ls -l` output indicates the file type:

| Character | Type |
|---|---|
| `-` | Regular file |
| `d` | Directory |
| `c` | Character device |
| `b` | Block device |
| `l` | Symbolic link |
| `s` | Socket |
| `p` | Named pipe (FIFO) |

```bash
ls -l /dev/sda      # b — block device
ls -l /dev/tty      # c — character device
ls -l /etc/passwd   # - — regular file
ls -l /tmp          # d — directory
ls -l /sbin/init    # l — symbolic link
```

### `file`

The `file` command identifies the type and format of any file:

```bash
file /etc/passwd       # ASCII text
file /bin/bash         # ELF 64-bit executable
file /dev/sda          # block special
file /dev/tty          # character special
```

---

## Regular Files

The most common type. Includes:

- Text files and configuration files
- Images and binaries
- Scripts and executables
- Databases

---

## Directories

Directories are special files that organise and contain other files and directories, forming the hierarchical Linux filesystem.

---

## Special Files

### Character Devices

Represent devices that process data **one character at a time**.

Examples: keyboard, serial ports, terminal devices.

Typically found under `/dev/` (e.g. `/dev/tty`, `/dev/null`).

### Block Devices

Represent devices that transfer data in **fixed-size blocks**.

Examples: hard drives, SSDs, NVMe drives, USB storage.

Typically found under `/dev/` (e.g. `/dev/sda`, `/dev/nvme0n1`).

### Links

**Hard Links** — multiple filenames pointing to the same inode. The underlying data persists until every hard link is removed.

```bash
ln original.txt hardlink.txt       # create a hard link
```

**Symbolic Links (Soft Links)** — store the path to another file, similar to a shortcut. Removing the symlink does not delete the original file.

```bash
ln -s /etc/nginx/nginx.conf nginx.conf    # create a symbolic link
```

> [!note] Hard links cannot span filesystems
> A hard link must reside on the same filesystem as the original file. Symbolic links have no such restriction.

### Socket Files

Enable **Inter-Process Communication (IPC)** between processes on the same machine. Commonly used by services such as databases and web servers (e.g. `/var/run/mysql/mysql.sock`).

### Named Pipes (FIFOs)

Allow one process to send data directly to another. Communication is **unidirectional** — data flows from writer to reader.

```bash
mkfifo mypipe                  # create a named pipe
echo "hello" > mypipe &        # write to it (in background)
cat mypipe                     # read from it
```
