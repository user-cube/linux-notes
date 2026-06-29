---
title: "07 - Package Management"
description: "Linux package management — package formats, package managers, and tools for Debian-based and RPM-based distributions."
tags:
  - linux
  - packages
  - apt
  - dnf
  - basics
---

Different Linux distributions use different package management systems. The two dominant package formats are:

| Package Format | Distributions |
|---|---|
| `.deb` | Debian, Ubuntu, Linux Mint |
| `.rpm` | Red Hat, CentOS, Fedora, Rocky Linux, AlmaLinux |

---

## What is a Package?

A package is a compressed archive containing everything required to install a piece of software:

- Executable binaries
- Shared libraries
- Configuration files
- Metadata (name, version, description)
- Dependency information

Because distributions may use different kernels, libraries, and package formats, the same application may require different packages depending on the operating system.

---

## Package Manager Responsibilities

A package manager automates the installation and management of packages. Its core responsibilities are:

- Installing, updating, and removing software
- Resolving and installing dependencies automatically
- Verifying package authenticity (digital signatures and checksums)
- Querying installed packages
- Searching available repositories

### Dependency Resolution

Package managers automatically install required dependencies. Without this, every library would need to be installed manually before the target package.

### Package Verification

Packages are verified using digital signatures and checksums to ensure they have not been modified and originate from a trusted source.

---

## Debian-Based Systems

The Debian ecosystem has three main tools, each at a different abstraction level.

### `dpkg`

Low-level package manager that works directly with `.deb` files.

```bash
dpkg -i package.deb       # install a local .deb file
dpkg -r package           # remove a package (keep config files)
dpkg -P package           # purge a package (remove config files too)
dpkg -l                   # list all installed packages
dpkg -s package           # show package status and metadata
```

> [!warning] No automatic dependency resolution
> `dpkg` does not fetch or install dependencies. Use `apt` for repository-managed installation.

### `apt`

Modern front-end built on top of `dpkg`. Automatically resolves dependencies and retrieves packages from configured repositories.

```bash
sudo apt update                  # refresh package index
sudo apt install nginx           # install a package
sudo apt remove nginx            # remove a package (keep config)
sudo apt purge nginx             # remove package and config files
sudo apt upgrade                 # upgrade all upgradable packages
sudo apt search nginx            # search available packages
apt show nginx                   # show package details
```

> [!tip] Always run `apt update` before installing
> The local package index can be stale. Running `apt update` first ensures you install the latest available version.

### `apt-get`

The traditional command-line interface for APT. Still widely available and preferred in scripts and automation because its output format is more stable and less likely to change between releases.

### APT vs APT-GET

| | `apt` | `apt-get` |
|---|---|---|
| Intended use | Interactive, day-to-day administration | Scripts, automation, legacy systems |
| Output | Cleaner, with progress bars | Minimal, verbose |
| Stability | Output format may change | Stable, consistent output |

> [!tip]
> Use `apt` at the terminal and `apt-get` in shell scripts — the stable output of `apt-get` means your scripts won't break if `apt` changes its formatting in a future release.

---

## RPM-Based Systems

Red Hat-based distributions use the `.rpm` package format.

### `rpm`

Low-level package manager for `.rpm` files.

```bash
rpm -ivh package.rpm    # install (verbose, progress hash marks)
rpm -Uvh package.rpm    # upgrade (or install if not present)
rpm -e package          # remove a package
rpm -q package          # query — check if a package is installed
rpm -qa                 # list all installed packages
rpm -qi package         # detailed package information
rpm -V package          # verify package integrity
```

> [!warning] No automatic dependency resolution
> Like `dpkg`, `rpm` does not resolve dependencies. Use `dnf` or `yum` for repository-managed installation.

### `yum`

YUM (Yellowdog Updater Modified) is a higher-level package manager built on top of RPM. It adds repository management and automatic dependency resolution.

```bash
sudo yum install nginx
sudo yum remove nginx
sudo yum update
```

> [!note] Legacy tool
> YUM is still available on older RHEL/CentOS 7 systems. On RHEL 8+ and Fedora, `yum` is often an alias for `dnf`.

### `dnf`

DNF (Dandified YUM) is the modern replacement for YUM, used by default on Fedora, RHEL 8+, Rocky Linux, and AlmaLinux.

```bash
sudo dnf install nginx
sudo dnf remove nginx
sudo dnf upgrade
sudo dnf search nginx
dnf info nginx
```

DNF improvements over YUM:

- Faster dependency resolution
- Lower memory usage
- Better error messages
- Plugin system with improved API
