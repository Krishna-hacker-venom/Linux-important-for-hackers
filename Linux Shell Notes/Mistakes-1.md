# Linux Shell Notes - My Mistakes and Lessons Learned

## Topic: Output Redirection (`>`), `echo`, `touch`, and `apropos`

---

# Mistake 1: Confusing `touch` as a command after `>`

### Command I typed

```bash
echo "hello krishna" > touch file.txt
```

### What I expected

* Create `file.txt`
* Write `hello krishna` into it

### What actually happened

Bash interpreted it as:

```bash
echo "hello krishna" file.txt > touch
```

Meaning:

* `echo` command
* Arguments:

  * `"hello krishna"`
  * `file.txt`
* Redirect output to a file named `touch`

The file **touch** received:

```text
hello krishna file.txt
```

### Why this happened

`>` expects **a filename**, **not a command**.

The shell never executes `touch` here.

After seeing `>`:

```bash
> touch
```

Bash immediately assumes:

> "touch" is the output file.

---

# Mistake 2: Using multiple `>` without understanding them

### Command

```bash
echo "hello krishna" > touch > file.txt
```

### What actually happened

Bash processed redirections from **left to right**.

Step 1

```bash
> touch
```

Output destination became:

```text
touch
```

Step 2

```bash
> file.txt
```

Output destination changed to:

```text
file.txt
```

### Final result

* `touch` was created (or emptied)
* `file.txt` received:

```text
hello krishna
```

### Lesson

The **last output redirection wins**.

---

# Mistake 3: Thinking `apropos` searches files

### Command

```bash
apropos krishna
```

### I expected

Search my files for the word "krishna".

### Reality

`apropos` searches the **manual page database (man pages)**.

It does **NOT** search files on disk.

Therefore:

```text
krishna: nothing appropriate.
```

means:

> No installed manual page contains the word "krishna".

---

# Mistake 4: Trying to search for a filename with `apropos`

### Command

```bash
apropos file.txt
```

### Why it failed

`apropos` searches command documentation.

It does not check whether `file.txt` exists.

---

# Correct Commands

## Create an empty file

```bash
touch file.txt
```

---

## Write text into a file

```bash
echo "hello krishna" > file.txt
```

---

## Append text

```bash
echo "hello again" >> file.txt
```

---

## Display file contents

```bash
cat file.txt
```

---

## Search inside a file

```bash
grep "krishna" file.txt
```

---

## Search manual pages

```bash
apropos copy
```

Example output:

```text
cp - copy files and directories
```

---

# How Bash Reads Commands

Whenever I write a command, I should mentally parse it in this order:

### Step 1 — Command

Example

```bash
echo
```

---

### Step 2 — Arguments

Example

```bash
"hello krishna"
```

---

### Step 3 — Redirections

Example

```bash
> file.txt
```

---

### Step 4 — Final destination

Ask myself:

* Is the output going to the terminal?
* A file?
* Another command through a pipe (`|`)?

---

# Mental Rule

Never read a shell command as one long sentence.

Instead, always split it into:

```text
Command
    ↓
Arguments
    ↓
Redirections
    ↓
Final Output Destination
```

---

# Common Beginner Mistakes

❌ Thinking `>` runs the command after it.

✔ `>` only specifies the **output file**.

---

❌ Thinking `apropos` searches files.

✔ `apropos` searches **manual pages**.

---

❌ Thinking `touch` after `>` is the `touch` command.

✔ After `>`, Bash treats the next word as a **filename**.

---

❌ Assuming every word after the command is another command.

✔ Unless separated by operators (`|`, `;`, `&&`, etc.), extra words become **arguments** to the current command.

---

# Key Takeaways

* The **first word** is usually the command.
* Words after the command are usually **arguments**.
* `>` redirects standard output to a **file**.
* The word immediately after `>` is interpreted as a **filename**.
* Multiple `>` operators are processed from left to right, and the **last one determines where the output goes**.
* `touch` creates an empty file.
* `echo` prints text.
* `cat` displays file contents.
* `grep` searches inside files.
* `apropos` searches manual page descriptions, **not** your filesystem.

---

# My Personal Reminder

Before pressing **Enter**, ask:

1. What is the command?
2. What are the arguments?
3. Is there any redirection (`>`, `>>`, `<`)?
4. Where will the output finally go?
5. Am I searching **files** (`grep`) or **manual pages** (`apropos`)?
