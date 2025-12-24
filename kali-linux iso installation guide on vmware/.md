# Kali Linux Full Installation on VMware Workstation Pro (Beginner Hacker Guide)

## 📌 Purpose of this Repository
This repository documents **step-by-step installation of Kali Linux using VMware Workstation Pro**, explained from a **beginner ethical hacker perspective**.

The goal is not just to install Kali, but to understand:
- Why each option is chosen
- How virtual machines work
- How to avoid common beginner mistakes (low disk, data loss, slow system)

---

## 🧠 Basic Concepts (Very Important)

### Host OS vs Guest OS
- **Host OS** → Your real operating system (Windows)
- **Guest OS** → Kali Linux running inside VMware

Kali runs in a **sandbox**, so:
- Your laptop hardware is safe
- Windows is untouched
- Mistakes won’t damage your system

This is why hackers use **Virtual Machines**.

---

## 🛠️ Tools Used

- **VMware Workstation Pro**
- **Kali Linux Installer ISO**
- Laptop with:
  - Minimum 8 GB RAM (recommended)
  - At least 80–100 GB free disk space

---

## 🚫 Common Beginner Mistake (IMPORTANT)

❌ Using **Live ISO / Try Kali**
- Very low disk space
- Changes not saved properly
- Tools fail to install
- Frequent “Low Disk Space” errors

✅ Solution: **Full Installation**

---

## 🚀 Step-by-Step Installation Guide

---

### Step 1: Create a New Virtual Machine
- Open **VMware Workstation Pro**
- Click **Create a New Virtual Machine**
- Select **Typical (Recommended)**

**Why?**
> Typical setup automatically configures safe defaults for beginners.

---

### Step 2: Select Kali Linux ISO
- Choose: `Installer disc image file (ISO)`
- Browse and select Kali Linux installer ISO

**Why Installer ISO?**
> Installer ISO creates a **real virtual hard disk**, unlike Live ISO.

---

### Step 3: Choose Guest OS
- OS: `Linux`
- Version: `Debian 12.x 64-bit`

**Why Debian?**
> Kali Linux is Debian-based, so this ensures compatibility.

---

### Step 4: Name & Location
- VM Name: `Kali Linux`
- Location: Drive with maximum free space

**Why?**
> Kali tools, wordlists, and logs consume a lot of disk.

---

### Step 5: Disk Configuration (CRITICAL STEP)

- Disk Size: **60 GB (minimum)**  
  **80 GB recommended**
- Select:  
  ✅ `Split virtual disk into multiple files`

**Why 60–80 GB?**
- Kali tools
- Updates
- Wordlists
- CTF files

**Why Split Disk?**
- Safer against corruption
- Easier to move or backup
- Performance difference is negligible

---

### Step 6: Hardware Customization

#### Recommended Settings:
- **RAM**: 4 GB (if laptop has 8 GB)
- **CPU**: 2 cores
- **Network**: NAT (default)

**Why not max RAM/CPU?**
> Over-allocating resources slows the host OS.

---

### Step 7: Start Installation
- Start VM
- Select:  
  `Graphical Install`

**Why Graphical?**
> Easier for beginners, same result as text install.

---

### Step 8: Language, Location, Keyboard
Choose according to preference.

---

### Step 9: Network Setup
- Use default network
- Hostname: `kali`
- Domain: leave empty

**Why default?**
> Safe and simple for learning.

---

### Step 10: User Creation
- Username: `kali`
- Password: strong but memorable

---

### Step 11: Disk Partitioning (MOST IMPORTANT)

Select:
1. `Guided – use entire disk`
2. `All files in one partition`
3. `Finish partitioning and write changes to disk`
4. Confirm **Yes**

**Why this option?**
- Simple
- No manual partition mistakes
- Best for beginners

---

### Step 12: Software Selection
Keep defaults selected:
- Kali Desktop
- Xfce
- Standard system utilities

---

### Step 13: GRUB Bootloader
- Install GRUB: **Yes**
- Device: `/dev/sda`

---

### Step 14: Finish Installation
- System reboots
- Remove ISO from VM settings

---

## ✅ Verification (Post-Install Check)

Open terminal and run:
```bash
df -h
