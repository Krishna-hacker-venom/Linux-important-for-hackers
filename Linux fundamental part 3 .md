

1️⃣ File & Command Location

🔍 locate

What it really does:
locate doesn’t search your disk live. It searches a ready-made list of all files.

Think of it like:

> Searching names in a phone contact list instead of walking door to door.



Why it’s fast

Because it looks in a database, not the disk.

Problem

If a file was created after the database update, locate won’t find it.

sudo updatedb
locate secret.txt

Hacker mindset

Good for quick recon on a system to see what files exist.


---

📍 whereis

Shows where the program really lives.

whereis python

Output may show:

/usr/bin/python

/usr/share/man/...


Why useful

Sometimes malware replaces commands.
If ls suddenly points somewhere strange → red flag 🚩


---

🧭 which

Tells:

> “When I type this command, which file actually runs?”



which ls

Security angle

Attackers sometimes put a fake command earlier in PATH.
which helps detect command hijacking.


---

🔎 find

This is the real detective.

Unlike locate, find:

Walks the disk live

Sees real-time files

Slower but accurate


find / -name "passwords.txt"

Real-world use

Find large log files

Find recently modified files

Find suspicious scripts



---

2️⃣ File & Directory Management

📁 mkdir

Creates folders.

mkdir project
mkdir -p a/b/c

-p means:

> “Create parents if they don’t exist.”




---

🔁 mv

Used for moving and renaming.

mv file.txt newname.txt

Linux thinks:

> Renaming = moving inside same folder.




---

❌ rm

Deletes files forever.

rm file.txt
rm -rf folder/

No recycle bin.
Once gone → only forensics might recover it.

⚠️ Hackers abuse rm to destroy logs.


---

📖 more

Reads big files page by page.

more /var/log/auth.log

Useful when reading:

Logs

Config files



---

3️⃣ DNS Tool

🌐 dig

Asks DNS servers questions.

dig google.com

It tells you:

IP address

Which DNS answered

How long it took


Security use

Check phishing domains

Track malicious infrastructure

Investigate command-and-control servers



---

4️⃣ File Permissions (explained simply)

Linux protects files using 3 layers:

Who	Meaning

User	File owner
Group	Team members
Others	Everyone else


Each has:

Read (r)

Write (w)

Execute (x)



---

🔐 chmod

Symbol way

chmod u+x script.sh

Means:

> Give user permission to run the file.




---

Number way

Permissions are numbers:

Permission	Value

Read	4
Write	2
Execute	1


Add them:

7 = rwx

6 = rw-

5 = r-x


chmod 755 app.sh

Means:

Owner: full power

Group: read + run

Others: read + run


Security meaning

Wrong permissions = privilege escalation risk.


---

👥 chgrp

Changes who shares access.

chgrp devteam app.sh

Now only that group can use it properly.

Used in:

Web servers

Dev teams

Secure environments



---

5️⃣ Process Management

A process = running program.


---

👀 See processes

ps aux
top

Shows:

CPU usage

Memory usage

Who started it


Hacker mindset

Look for:

Strange names

High CPU for no reason

Programs running as root



---

🛑 Kill process

kill PID
kill -9 PID

kill → polite request

kill -9 → force shutdown


Used when:

Malware runs

Program freezes

Crypto miners hide



---

6️⃣ Scheduling Processes

⏰ cron

Runs jobs automatically.

crontab -e

Example:

0 2 * * * /backup.sh

Means:

> Run every day at 2 AM



Security reality

Attackers use cron for:

Persistence

Re-running malware

Log wiping


If you find unknown cron jobs → system may be compromised.


---

🕒 at

Run once in future.

at 18:00

Good for:

One-time tasks

Delayed scripts



---

7️⃣ Mounting & Unmounting

💽 What is mounting?

Linux doesn’t auto-open disks.
You must attach them to a folder.


---

🔗 mount

mount /dev/sdb1 /mnt/usb

Means:

> Connect this device to this folder.



Now files appear inside /mnt/usb.


---

🔌 umount

umount /mnt/usb

Safely disconnects it.


---

🧠 /etc/fstab

Auto-mount file.

System reads it on boot:

> “These disks should always attach here.”




---

8️⃣ How all this fits Ethical Hacking

Command	Real security use

find	Locate malware, backdoors
chmod	Lock down sensitive scripts
chgrp	Role-based access
ps	Detect hidden miners
dig	Trace phishing domains
cron	Detect persistence
mount	Mount forensic images



---

Final mindset

You’re not just learning commands.
You’re learning how Linux thinks.
