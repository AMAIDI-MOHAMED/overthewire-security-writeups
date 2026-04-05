# Bandit Level 9 → Level 10

## 🎯 Goal

The password for the next level is stored in the file `data.txt` in one of the few human-readable strings, preceded by several `=` characters.

**Connect with:** `ssh bandit9@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How to extract human-readable strings from binary files using the `strings` command
- How to pipe `strings` output into `grep` to filter results
- Why binary files contain readable text embedded within them
- How the `=` pattern helps identify the password line

---

## 🛠 Commands Used

```bash
# Extract all human-readable strings from the binary file
strings data.txt

# Pipe into grep to find lines preceded by "=" characters
strings data.txt | grep "==="
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Check the file type**

```bash
file data.txt
```

Output:
```
data.txt: data
```

It's a binary file. Opening it with `cat` would produce garbled output and mess up your terminal. Instead, use `strings`.

**Step 2 — Use `strings` to extract readable text**

The `strings` command scans a binary file and prints any sequence of printable characters that is at least 4 characters long. It's designed for extracting text from executables and data files.

```bash
strings data.txt
```

This produces many lines — some readable, some garbage. We need to find the line preceded by `=` signs.

**Step 3 — Filter with `grep`**

```bash
strings data.txt | grep "==="
```

This filters to only lines containing `===`. One of those lines will be the password, looking something like:

```
========== thepassword123
```

The password is the string after the `=` characters.

---

## 🔐 Cybersecurity / SOC Relevance

`strings` is one of the first tools used in basic malware analysis. When a suspicious binary is found on a system, analysts run `strings` to quickly extract:
- **Hardcoded URLs or IP addresses** — command and control (C2) server addresses
- **Hardcoded credentials** — usernames, passwords, API keys embedded by the developer
- **Error messages and function names** — clues about what the malware does
- **Registry keys or file paths** — artifacts of the malware's behavior on Windows systems

This technique is part of **static analysis** — examining a file without executing it — which is the safest first step when triaging suspected malware.

---

## 💡 Key Takeaways

- `strings` extracts human-readable text from binary files
- Combine `strings` with `grep` to filter for specific patterns
- Binary files can contain embedded readable text — that's what `strings` targets
- `strings -n 8` increases the minimum string length (default is 4) to reduce noise

---

## ❌ Common Mistakes

- **Running `cat` on the binary file** — this produces garbled output and can corrupt your terminal display (run `reset` to fix a corrupted terminal)
- **Not piping into `grep`** — `strings` output alone has too many lines to scan manually
- **Searching for only one `=`** — the hint says "several `=` characters," so `grep "==="` or `grep "=="` narrows it down well
