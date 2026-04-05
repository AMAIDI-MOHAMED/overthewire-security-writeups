# 🔐 Bandit Level 4

## 🎯 Goal

There are multiple files in the `inhere` directory named `-file00` through `-file09`. Only one contains human-readable text (the password). Find it.

- **Login:** `ssh bandit4@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 3)*

---

## 🧠 What I Learned

- How to use the `file` command to determine a file's type without opening it
- The difference between ASCII text and binary data
- How to efficiently search through multiple files

---

## 🛠 Commands Used

```bash
cd inhere
ls
file ./-file0*
cat ./-file07
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Navigate and list files**

```bash
cd inhere
ls -la
```

Output:
```
-file00  -file01  -file02  -file03  -file04
-file05  -file06  -file07  -file08  -file09
```

Ten files, all with the `-` prefix (so we need `./` to reference them).

**Step 2 — Use `file` to identify types**

Rather than opening each one (binary files can print garbage or mess up the terminal), use `file` to identify their types:

```bash
file ./-file0*
```

Output:
```
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```

Only `./-file07` is `ASCII text` — that's our target.

**Step 3 — Read the human-readable file**

```bash
cat ./-file07
```

This outputs the password for Level 5.

---

## 🔐 Cybersecurity / SOC Relevance

The `file` command is a fundamental tool in **digital forensics** and **malware analysis**:

- Attackers often rename malicious executables with innocent extensions (e.g., `document.pdf` that is actually an ELF binary)
- `file` reads the **magic bytes** at the start of a file — not just the extension — making it reliable
- SOC Analysts and malware analysts use this technique when triaging suspicious files from phishing emails or incident scenes

Relying on file extensions alone is a dangerous assumption in security work.

---

## 💡 Key Takeaways

- `file <filename>` reads magic bytes to determine the true file type
- Wildcards (`*`) let you run a command on multiple files at once
- Human-readable text is ASCII — binary data contains non-printable characters
- Never trust file extensions for security-critical decisions

---

## ❌ Common Mistakes

- Trying to `cat` all files — binary files can flood the terminal with garbage
- Forgetting the `./` prefix when files start with `-`
- Running `file -file0*` without `./` — treated as flags
