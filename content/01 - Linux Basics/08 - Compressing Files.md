---
title: "08 - Compressing Files"
description: "Linux compression utilities — gzip, bzip2, and xz — and reading compressed files without extracting them."
tags:
  - linux
  - compression
  - basics
---

Linux includes several built-in utilities for compressing files. The three most common are:

| Tool | Extension | Speed | Compression ratio |
|---|---|---|---|
| `gzip` | `.gz` | Fast | Moderate |
| `bzip2` | `.bz2` | Moderate | Better than gzip |
| `xz` | `.xz` | Slow | Best |

> [!note] Source file behaviour
> By default, all three tools **replace** the original file with the compressed version. Use the `-k` flag to keep the original file alongside the compressed copy.

---

## `gzip`

```bash
gzip file.txt          # compress — produces file.txt.gz, removes file.txt
gzip -k file.txt       # compress and keep the original
gzip -9 file.txt       # maximum compression level (1=fastest, 9=best)
gunzip file.txt.gz     # decompress
```

---

## `bzip2`

```bash
bzip2 file.txt         # compress — produces file.txt.bz2
bzip2 -k file.txt      # compress and keep the original
bunzip2 file.txt.bz2   # decompress
```

---

## `xz`

```bash
xz file.txt            # compress — produces file.txt.xz
xz -k file.txt         # compress and keep the original
xz -9 file.txt         # maximum compression level
unxz file.txt.xz       # decompress
```

---

## Reading Compressed Files Without Extracting

Linux provides utilities to view the contents of compressed files directly, without creating a decompressed copy on disk.

| Format | Command |
|---|---|
| `.gz` | `zcat file.gz` |
| `.bz2` | `bzcat file.bz2` |
| `.xz` | `xzcat file.xz` |

```bash
zcat access.log.gz | grep "404"     # search inside a compressed log
bzcat archive.bz2 | head -20        # view the first 20 lines
```

---

## Archives with `tar`

Compression tools only compress a single file at a time. `tar` is used to bundle multiple files into a single archive, often combined with a compression tool.

```bash
# Create a compressed archive
tar -czf archive.tar.gz  /path/to/dir    # gzip
tar -cjf archive.tar.bz2 /path/to/dir   # bzip2
tar -cJf archive.tar.xz  /path/to/dir   # xz

# Extract a compressed archive
tar -xzf archive.tar.gz
tar -xjf archive.tar.bz2
tar -xJf archive.tar.xz

# List contents without extracting
tar -tzf archive.tar.gz
```

| Flag | Meaning |
|---|---|
| `-c` | Create archive |
| `-x` | Extract archive |
| `-t` | List contents |
| `-z` | Use gzip |
| `-j` | Use bzip2 |
| `-J` | Use xz |
| `-f` | Specify filename |
| `-v` | Verbose output |
