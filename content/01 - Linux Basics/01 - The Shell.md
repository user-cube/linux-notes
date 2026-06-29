---
title: "01 - The Shell"
description: "What the shell is, directory navigation with pushd/popd, environment variables, PATH, and shell prompt customisation."
tags:
  - linux
  - shell
  - basics
---

The **shell** is a program that provides a text-based interface between the user and the operating system.

When a user logs into a Linux system, they are placed in their **home directory**:

```bash
/home/<username>
```

The home directory can also be represented with the tilde symbol: `~`

---

## Directory Navigation — `pushd` and `popd`

`pushd` saves the current working directory onto a **directory stack** and changes to another directory. `popd` returns you to the previously saved directory.

```bash
pushd /etc   # save current dir, move to /etc
cd /var
cd /tmp
popd         # returns to /etc
```

> [!tip] Useful for multi-directory admin tasks
> `pushd`/`popd` are especially handy during administrative tasks where you need to navigate to a temporary location and quickly return to where you were.

---

## Environment Variables

Environment variables store information accessible by the shell and by applications.

### Local Shell Variable

```bash
OFFICE=Calston
```

- Exists only in the **current shell session**
- **Not** inherited by child processes or programs launched from the shell

### Environment Variable

```bash
export OFFICE=Calston
```

Using `export` makes the variable available to:

- The current shell
- Child processes
- Programs launched from the current shell

### Inspecting Variables

```bash
env               # list all current environment variables
printenv OFFICE   # print the value of a specific variable
echo $OFFICE      # also prints the value
```

---

## Making Variables Persistent

Variables set in the terminal are lost when the session ends. To persist them across logins, add them to a shell startup file:

| File | When loaded |
|---|---|
| `~/.profile` | Login shells (most distributions) |
| `~/.bashrc` | Interactive bash shells |
| `~/.zshrc` | Interactive zsh shells |

> [!warning] `~/.pam_environment` is deprecated
> This file was used for PAM-based login in older distributions but has been **deprecated since Ubuntu 20.04 / PAM 1.4** and should not be used on modern systems.

---

## Extending `PATH`

`PATH` is a special environment variable that tells the shell where to look for executables. New locations can be added without replacing the existing ones:

```bash
export PATH=$PATH:/path/to/bin
```

This allows executables in `/path/to/bin` to be run without specifying their full path.

---

## Customising the Shell Prompt — `PS1`

The shell prompt appearance is controlled by the `PS1` environment variable:

```bash
echo $PS1
```

By modifying `PS1`, you can include information such as username, hostname, current directory, date and time, and colours.

> [!note] Escape codes are shell-specific
> The escape codes used in `PS1` differ between shells:
>
> | Info | bash | zsh |
> |---|---|---|
> | Username | `\u` | `%n` |
> | Hostname | `\h` | `%m` |
> | Current directory | `\w` | `%~` |
>
> Colours use ANSI escape codes in both shells, but the syntax for embedding them also differs.

> [!tip] Persist prompt changes
> Add your custom `PS1` to `~/.bashrc` (bash) or `~/.zshrc` (zsh) to keep it across sessions.
