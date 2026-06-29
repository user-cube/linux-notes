---
title: "11 - The vi Editor"
description: "The vi/vim text editor — modes, navigation, editing commands, search, and saving."
tags:
  - linux
  - vi
  - vim
  - basics
---

`vi` (and its enhanced version, `vim`) is one of the most widely used text editors in Linux. It is available on virtually every Unix-like system, making it a valuable skill for system administration and remote work.

```bash
vi filename     # open a file (creates it if it does not exist)
vim filename    # open with vim (extended vi)
```

---

## Editing Modes

When vi starts, it opens in **Command Mode**. Key presses are interpreted as editor commands, not text input.

| Mode | How to enter | Purpose |
|---|---|---|
| **Command Mode** | Default / press `Esc` | Navigation, copying, deleting, searching |
| **Insert Mode** | Press `i` | Enter and edit text |
| **Last-Line Mode** | Press `:` | Save, quit, search and replace |

> [!tip] Press `Esc` to return to Command Mode from any other mode.

---

## Navigation (Command Mode)

| Key | Action |
|---|---|
| `h` | Move left |
| `j` | Move down |
| `k` | Move up |
| `l` | Move right |
| `w` | Jump to the start of the next word |
| `b` | Jump to the start of the previous word |
| `0` | Move to the beginning of the line |
| `$` | Move to the end of the line |
| `gg` | Go to the first line of the file |
| `G` | Go to the last line of the file |
| `:<n>` | Go to line number `n` (e.g. `:42`) |

---

## Editing (Command Mode)

| Command | Description |
|---|---|
| `i` | Enter Insert Mode before the cursor |
| `a` | Enter Insert Mode after the cursor |
| `o` | Open a new line below and enter Insert Mode |
| `x` | Delete the character under the cursor |
| `dd` | Delete the current line |
| `3dd` | Delete three lines |
| `yy` | Copy (yank) the current line |
| `p` | Paste after the cursor |
| `u` | Undo the last change |
| `Ctrl + r` | Redo an undone change |

---

## Search (Command Mode)

```
/pattern        # search forward for pattern
?pattern        # search backward for pattern
n               # jump to the next match
N               # jump to the previous match
```

---

## Last-Line Mode Commands

Enter Last-Line Mode by pressing `:` in Command Mode.

| Command | Description |
|---|---|
| `:w` | Save the file |
| `:q` | Quit (fails if there are unsaved changes) |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |
| `:set number` | Show line numbers |
| `:set nonumber` | Hide line numbers |
| `:%s/old/new/g` | Replace all occurrences of `old` with `new` |
| `:%s/old/new/gc` | Same, with confirmation before each replacement |

---

## Quick Save and Exit

In Command Mode:

```
ZZ    # save (if modified) and quit
ZQ    # quit without saving (equivalent to :q!)
```
