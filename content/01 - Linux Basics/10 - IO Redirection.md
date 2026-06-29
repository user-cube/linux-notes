---
title: "10 - IO Redirection"
description: "Standard streams, output and input redirection, /dev/null, pipes, and the tee command."
tags:
  - linux
  - shell
  - basics
---

Every Linux process communicates through three standard data streams:

| Stream | Description | File Descriptor |
|---|---|---|
| **stdin** | Input received by the program | 0 |
| **stdout** | Normal program output | 1 |
| **stderr** | Error messages | 2 |

These streams can be redirected to files or other commands.

---

## Output Redirection

```bash
ls > files.txt          # stdout → file (overwrite)
ls >> files.txt         # stdout → file (append)
ls 2> errors.txt        # stderr → file (overwrite)
ls 2>> errors.txt       # stderr → file (append)
ls > output.txt 2>&1    # stdout and stderr → same file
ls &> output.txt        # shorthand for stdout and stderr → same file (bash)
```

> [!note] Order matters with `2>&1`
> `command > file 2>&1` redirects stderr to wherever stdout currently points (the file).
> `command 2>&1 > file` redirects stderr to the terminal (where stdout was before the redirect), then redirects stdout to the file — not the same thing.

---

## Input Redirection

```bash
sort < names.txt        # read stdin from a file instead of the keyboard
```

---

## `/dev/null`

A special device that silently discards everything written to it. Useful for suppressing unwanted output.

```bash
command > /dev/null             # discard stdout
command 2> /dev/null            # discard stderr
command > /dev/null 2>&1        # discard both stdout and stderr
```

---

## Pipes

The pipe operator (`|`) connects the **stdout** of one command to the **stdin** of the next.

```bash
ls -l | grep ".txt"                          # filter ls output
cat logfile | grep ERROR | sort | uniq       # chain multiple commands
ps aux | grep nginx                          # find running processes
```

Pipes are one of the most powerful features of the Linux command line — each program does one job, and pipes compose them into complex workflows.

---

## `tee`

`tee` writes output to **both** the terminal and a file simultaneously. Useful when you want to save output while still seeing it on screen.

```bash
ls | tee files.txt          # write to terminal and file (overwrite)
ls | tee -a files.txt       # write to terminal and file (append)
```

Unlike plain redirection (`>`), `tee` lets you inspect the output in real time while also capturing it.

```bash
make 2>&1 | tee build.log   # capture build output (stdout + stderr) and display it live
```
