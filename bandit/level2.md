# 🔐 Bandit Level 2

## 🎯 Goal

Read a file named `spaces in this filename` to retrieve the password for Level 3.

- **Login:** `ssh bandit2@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 1)*

---

## 🧠 What I Learned

- How spaces in filenames cause issues with shell parsing
- Two ways to handle filenames with spaces: quoting and escaping
- Why filenames with spaces are problematic in scripts and automation

---

## 🛠 Commands Used

```bash
ls
cat "spaces in this filename"
# or equivalently:
cat spaces\ in\ this\ filename
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — List files**

```bash
ls
```

Output:
```
spaces in this filename
```

**Step 2 — Understand the problem**

The shell splits command arguments on whitespace. If you type:

```bash
cat spaces in this filename
```

The shell tries to open four separate files: `spaces`, `in`, `this`, and `filename` — none of which exist.

**Step 3 — Quote or escape the filename**

**Option A — Use quotes:**
```bash
cat "spaces in this filename"
```

**Option B — Escape each space with a backslash:**
```bash
cat spaces\ in\ this\ filename
```

**Option C — Use tab-completion:**
Type `cat sp` then press `Tab`. The shell will autocomplete the filename and handle escaping automatically.

This outputs the password for Level 3.

---

## 🔐 Cybersecurity / SOC Relevance

Filenames with spaces are frequently used in:

- **Malware delivery**: Attackers place malicious scripts in directories or with filenames that break naive shell parsing
- **Log evasion**: File paths with spaces can confuse log parsers that split fields on whitespace
- **Forensic analysis**: When examining a compromised system, analysts may encounter files with unusual names meant to deter investigation

Understanding how to handle these cases makes forensic and incident-response work more reliable.

---

## 💡 Key Takeaways

- Always quote filenames in shell scripts to handle spaces safely: `"$filename"`
- Tab-completion in bash is a powerful way to safely handle special filenames
- Prefer `_` (underscores) over spaces in filenames in your own projects — it avoids these headaches

---

## ❌ Common Mistakes

- Typing `cat spaces in this filename` without quotes → 4 separate "file not found" errors
- Forgetting to escape spaces when constructing shell scripts dynamically
