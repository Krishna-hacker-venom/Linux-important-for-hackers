# Linux fundamental for hackers
## 1️⃣ Why Hackers MUST Know Linux

Linux gives you:
- Full visibility of the system
- Powerful text processing
- Scriptable automation
- Transparent permission model
- Weak configurations everywhere

Most real-world hacks end like this:

```bash
$ whoami
www-data
````

If you don’t know Linux — **the attack ends here**.

---

# 🔹 BASIC LINUX COMMANDS (DEEP DIVE)

---

## `echo` — Control Output, Payloads & Scripts

### What it does

Prints text to standard output.

### Why hackers care

* Create payloads
* Write files
* Inject data
* Debug scripts
* Bypass filters

### Examples

```bash
echo "test"
echo $PATH
echo $(whoami)
```

### File creation (VERY IMPORTANT)

```bash
echo "malicious content" > file.txt
```

Overwrite vs append:

```bash
>   overwrite
>>  append
```

### Real-world attacker use

```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

💀 **This single command creates a webshell.**

---

## `whoami` — Identity Awareness

### What it does

Shows current effective user.

### Why hackers care

Permissions decide:

* What you can read
* What you can write
* What you can execute
* Whether privilege escalation is possible

### Example

```bash
whoami
```

Common outputs:

* `www-data` → web server user
* `user` → limited shell
* `root` → game over

### Attacker mindset

Every exploit begins with:

```bash
whoami
id
```

---

## `ls` — Reconnaissance of Files

### What it does

Lists directory contents.

### Why hackers care

Files = secrets.

### Important flags

```bash
ls -l     # permissions
ls -a     # hidden files
ls -la    # everything
```

### Permissions example

```bash
-rw-r--r-- 1 root root config.php
```

Interpretation:

* Owner: root
* Readable by everyone ❌ (bad config)

### Hacker use

* Find `.env`
* Find backups
* Find SSH keys
* Find scripts run by cron

---

## `cd` — Navigation is Control

### What it does

Changes directory.

### Why hackers care

Privilege escalation often depends on **where you are**.

```bash
cd /
cd /var/www
cd /home
cd /tmp
```

### Dangerous directories

* `/tmp` → world writable
* `/var/www` → web root
* `/etc` → configs
* `/home/*` → user data

---

## `cat` — Read the Secrets

### What it does

Displays file contents.

### Why hackers care

Linux stores **everything as text**.

```bash
cat config.php
cat /etc/passwd
cat /etc/crontab
```

### Real-world secrets found via `cat`

* Database passwords
* API keys
* JWT secrets
* SMTP creds
* Hardcoded tokens

---

## `pwd` — Situational Awareness

### What it does

Prints current directory.

### Why hackers care

* Know your execution context
* Payload paths depend on it
* Cron & scripts rely on absolute paths

```bash
pwd
```

Never guess — always confirm.

---

## `find` — Weaponized Search

### What it does

Searches files and directories.

### Why hackers care

`find` is **privilege escalation gold**.

### Common hacker patterns

#### Find SUID binaries

```bash
find / -perm -4000 2>/dev/null
```

#### Find writable files

```bash
find / -writable 2>/dev/null
```

#### Find config files

```bash
find / -name "*.conf" 2>/dev/null
```

#### Find secrets

```bash
find / -name ".env" 2>/dev/null
```

This command alone finds **80% of Linux privesc bugs**.

---

## `file` — Identify Real File Types

### What it does

Detects file type by content, not extension.

### Why hackers care

Attackers don’t trust extensions.

```bash
file backup
file image.jpg
```

### Example

```bash
image.jpg: PHP script, ASCII text
```

🔥 This is how file upload bypasses are confirmed.

---

## `grep` — Pattern Hunting

### What it does

Searches inside files.

### Why hackers care

Secrets hide in text.

```bash
grep "password" config.php
grep -R "API_KEY" /
grep -R "token" .
```

Recursive search:

```bash
grep -Ri "secret" /var/www
```

### Real-world bug bounty use

* Find hardcoded creds
* Find debug flags
* Find admin endpoints

---

## `wc` — Data Awareness

### What it does

Counts lines, words, bytes.

### Why hackers care

* Log analysis
* Payload size checks
* Wordlist analysis

```bash
wc -l passwords.txt
wc -c payload.bin
```

---

# 🔹 LINUX FILE SYSTEM (ATTACKER VIEW)

---

## Linux Philosophy

> Everything is a file.

---

## Critical Directories

| Directory         | Why Hackers Care   |
| ----------------- | ------------------ |
| `/`               | Root of system     |
| `/etc`            | Configs, passwords |
| `/home`           | User data          |
| `/var/www`        | Web apps           |
| `/var/log`        | Logs = intel       |
| `/tmp`            | World writable     |
| `/bin` `/usr/bin` | Binaries           |
| `/root`           | Root secrets       |

---

## `/etc/passwd` vs `/etc/shadow`

```bash
cat /etc/passwd
```

If shadow is readable → **instant privesc**.

---

# 🔹 SHELL OPERATORS (POWER COMBOS)

---

## `|` (Pipe) — Chain Commands

```bash
cat access.log | grep "admin"
```

Attackers **never use single commands** — they chain.

---

## `>` and `>>` — File Control

```bash
echo test > file.txt
echo test >> file.txt
```

Overwrite vs append.

---

## `&&` — Logical AND

```bash
cd /var/www && ls
```

Only runs if first command succeeds.

---

## `;` — Command Injection Gold

```bash
ping 127.0.0.1; whoami
```

🔥 **This is classic command injection**.

---

## `$( )` — Command Substitution

```bash
echo $(whoami)
```

Used heavily in:

* RCE
* Payload crafting
* Filter bypass

---

## `2>/dev/null` — Hide Errors

```bash
find / -perm -4000 2>/dev/null
```

Clean output = better analysis.

---

# 🧠 Final Hacker Mindset

Linux is not about memorizing commands.

It is about:

* Understanding **permissions**
* Understanding **execution**
* Understanding **trust boundaries**
* Understanding **misconfigurations**




