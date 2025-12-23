# 🔍 Linux Privilege Escalation – `find` Command (THM)

This document explains the **Linux `find` command** from a **Privilege Escalation (PrivEsc)** point of view, in **two formats**:

1. **General Way** – simple, real-life, Hinglish explanation
2. **Technical Way** – Linux-focused with commands and attacker mindset

---

## 🧠 Core Idea of `find`

### 🔹 General Way

`find` command = **system ko scan karne wali aankhen 👀**

Iska use karke hum dhundhte hain:

* Galat permissions
* Root-owned scripts
* SUID binaries
* Writable folders/files
* Recently changed files

> Privilege Escalation ka 80% part = **Enumeration**

---

### 🔹 Technical Way

`find` recursively filesystem scan karta hai based on:

* name
* permission
* owner
* time
* size
* type

**Syntax:**

```bash
find <start_path> <conditions>
```

---

## 📁 File & Directory Search

### 1️⃣ Find File by Name

#### General Way

Agar tumhe lagta hai koi **important file (flag, password, config)** exist karti hai, tum naam se search karte ho.

#### Technical Way

```bash
find . -name flag1.txt
find /home -name flag1.txt
```

> Case-insensitive search:

```bash
find / -iname flag1.txt
```

---

### 2️⃣ Find Directory by Name

#### General Way

Config directories me aksar **credentials & secrets** hote hain.

#### Technical Way

```bash
find / -type d -name config
```

---

## 🔐 Permission-Based Enumeration (PrivEsc Gold)

### 3️⃣ World-Writable Files (777)

#### General Way

Agar koi file **sab users ke liye writable** hai, tum usme apna malicious code daal sakte ho.

#### Technical Way

```bash
find / -type f -perm 0777
```

---

### 4️⃣ Executable Files

#### General Way

Executable files agar root ke under chal rahi hain → exploit possible.

#### Technical Way

```bash
find / -perm a=x
```

---

### 5️⃣ Files by Owner

#### General Way

Specific user (jaise `frank`) ke files suspicious ho sakte hain.

#### Technical Way

```bash
find /home -user frank
```

---

## ⏱️ Time-Based Enumeration

### 6️⃣ Recently Modified Files

#### General Way

Recently modified files aksar:

* scripts
* cron jobs
* backups

#### Technical Way

```bash
find / -mtime 10
```

---

### 7️⃣ Recently Accessed Files

#### General Way

Recently accessed files me **passwords ya logs** ho sakte hain.

#### Technical Way

```bash
find / -atime 10
```

---

### 8️⃣ Recently Changed (Minutes Level)

#### General Way

Agar abhi-abhi koi file change hui hai → system ka kaam chal raha hai.

#### Technical Way

```bash
find / -cmin -60
find / -amin -60
```

---

## 📦 File Size Enumeration

### 9️⃣ Large Files

#### General Way

Large files = backups, logs, dumps → sensitive info ho sakti hai.

#### Technical Way

```bash
find / -size 50M
find / -size +100M
find / -size -5M
```

---

## 🚫 Error Suppression (Clean Output)

### General Way

`find` bahut saare **Permission denied** errors deta hai.

### Technical Way

```bash
2>/dev/null
```

Example:

```bash
find / -type f -perm 0777 2>/dev/null
```

---

## ✍️ Writable Directories (High Risk)

### 🔟 World-Writable Directories

#### General Way

Writable directory me tum:

* files create kar sakte ho
* scripts replace kar sakte ho

#### Technical Way (Equivalent Commands)

```bash
find / -writable -type d 2>/dev/null
find / -perm -222 -type d 2>/dev/null
find / -perm -o w -type d 2>/dev/null
```

---

### 🔟 World-Executable Directories

#### General Way

Executable directory = system yahan se commands run karega.

#### Technical Way

```bash
find / -perm -o x -type d 2>/dev/null
```

---

## 🛠️ Development Tools Enumeration

### 1️⃣1️⃣ Programming Languages

#### General Way

Python / Perl / GCC mil gaya → exploit likhna easy.

#### Technical Way

```bash
find / -name python*
find / -name perl*
find / -name gcc*
```

---

## 🔥 SUID Files (Privilege Escalation King)

### 1️⃣2️⃣ SUID Bit Files

#### General Way

SUID file bolti hai:

> "Mujhe chalao, mai root ban ke chalungi"

#### Technical Way

```bash
find / -perm -u=s -type f 2>/dev/null
```

---

## 🧠 Attacker Mindset

### General Thinking

* Kaunsi cheez root ke under chal rahi hai?
* Kaunsi cheez mai modify kar sakta hoon?

### Technical Thinking

* Writable + root execution = 🔥
* SUID + exploitable binary = 🔥
* Cron + writable script = 🔥

---

## ✅ THM Practical Enumeration Flow

```bash
find / -perm -u=s -type f 2>/dev/null
find / -writable -type d 2>/dev/null
find / -type f -perm 0777 2>/dev/null
find / -mtime 1
```

---

### 🎯 Summary

* `find` is the **most powerful enumeration command** in Linux PrivEsc
* Always think: **Who owns it? Who can write it? Who executes it?**

---

💡 *Use this document while solving TryHackMe Linux Privilege Escalation rooms.*
