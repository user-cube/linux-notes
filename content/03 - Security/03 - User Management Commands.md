---
title: "03 - User Management Commands"
description: "Linux commands for creating, modifying, and deleting user accounts and groups."
tags:
  - linux
  - security
  - users
---

Linux provides dedicated commands for managing user accounts and groups. These commands safely update the underlying system files (`/etc/passwd`, `/etc/shadow`, `/etc/group`) and should always be used instead of editing those files manually.

---

## User Commands

### `useradd`

Creates a new user account.

```bash
sudo useradd john                              # create user (no home directory)
sudo useradd -m john                           # create user with home directory
sudo useradd -m -s /bin/bash john             # specify login shell
sudo useradd -m -G sudo,developers john       # add to supplementary groups at creation
```

> [!note] `useradd` vs `adduser`
> On Debian/Ubuntu, `adduser` is a friendlier interactive wrapper around `useradd` — it prompts for a password and GECOS information and creates the home directory by default. `useradd` is the lower-level POSIX command available on all distributions.

### `passwd`

Sets or changes a user's password.

```bash
sudo passwd john     # set/change john's password (as root)
passwd               # change your own password (no argument needed)
```

### `usermod`

Modifies an existing user account without deleting and recreating it.

```bash
sudo usermod -aG sudo john           # add john to the sudo group (-a = append, required)
sudo usermod -s /bin/zsh john        # change login shell
sudo usermod -l newname john         # rename the account
sudo usermod -L john                 # lock the account (disable login)
sudo usermod -U john                 # unlock the account
```

> [!warning] Always use `-aG`, never `-G` alone
> `usermod -G sudo john` **replaces** all supplementary groups with just `sudo`. Using `-aG` appends to the existing group list.

### `userdel`

Removes a user account.

```bash
sudo userdel john          # remove the account only
sudo userdel -r john       # remove the account and home directory
```

### `whoami`

Displays the username of the currently active user session.

```bash
whoami
```

Useful when working with multiple accounts or after switching users with `su`.

---

## Group Commands

### `groupadd`

Creates a new group.

```bash
sudo groupadd developers
sudo groupadd -g 1500 developers     # specify a GID explicitly
```

### `groupdel`

Deletes an existing group.

```bash
sudo groupdel developers
```

> [!note]
> A group cannot be removed if it is the **primary group** of any existing user. Remove or reassign those users first.

---

## Quick Reference

| Command | Purpose |
|---|---|
| `useradd -m username` | Create user with home directory |
| `passwd username` | Set or change password |
| `usermod -aG group username` | Add user to a group |
| `userdel -r username` | Remove user and home directory |
| `groupadd groupname` | Create a group |
| `groupdel groupname` | Delete a group |
| `id username` | Show UID, GID, and group membership |
| `groups username` | List groups a user belongs to |
