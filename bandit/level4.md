# Bandit Level 4 → Level 5

## 🎯 Goal

The password for the next level is stored in the only human-readable file in the `inhere` directory. There are 10 files named `-file00` through `-file09`.

**Connect with:** `ssh bandit4@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How to determine a file's type using the `file` command
- The difference between binary (non-human-readable) and ASCII text files
- How to use wildcards (`*`) to apply a command to multiple files at once
- Efficient investigation when many files are present

---

## 🛠 Commands Used

```bash
# Navigate to the inhere directory
cd inhere

# List all files
ls -la

# Check the type of all files at once using a wildcard
file ./-file0*

# Read the human-readable file (the one identified as ASCII text)
cat ./-file07
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Connect and navigate**

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
cd inhere
ls
```

Output:
```
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```

Ten files. Opening each one manually would be slow and messy.

**Step 2 — Use the `file` command**

The `file` command examines the content of a file and reports what type of data it contains — it does NOT rely on the file extension.

```bash
file ./-file0*
```

The `*` wildcard matches any characters, so `./-file0*` expands to all ten files. The `./` prefix avoids the special `-` name issue covered in Level 1.

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

Only `./-file07` is **ASCII text** — human-readable. All others are binary data.

**Step 3 — Read the file**

```bash
cat ./-file07
```

The password for `bandit5` is displayed.

---

## 🔐 Cybersecurity / SOC Relevance

The `file` command is a fundamental tool in malware analysis and digital forensics. Attackers frequently rename malicious executables with innocent extensions (e.g., `report.pdf`) to trick users or analysts. The `file` command reads the **magic bytes** (the first few bytes of a file) to determine the true file type, regardless of extension.

As a SOC Analyst, this skill is used when:
- Analyzing suspicious email attachments
- Triaging files found on a compromised host
- Investigating uploads to web servers that might bypass extension-based filters

---

## 💡 Key Takeaways

- `file <filename>` identifies the true type of a file based on its content, not its name
- Wildcards like `*` let you apply commands to multiple files at once — very efficient
- "Human-readable" means ASCII or UTF-8 text; "data" typically means binary content
- Always use `./` before filenames starting with `-` to avoid shell misinterpretation

---

## ❌ Common Mistakes

- **Opening each file manually with `cat`** — binary files will produce garbled output and is inefficient
- **Trusting file extensions** — a `.txt` extension doesn't guarantee readable content
- **Forgetting `./` before filenames** — `-file07` looks like a flag; `./-file07` is a path
