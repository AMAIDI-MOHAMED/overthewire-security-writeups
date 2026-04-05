# 🔐 Bandit Level 3

## 🎯 Goal

Find a hidden file inside the `inhere` directory to retrieve the password for Level 4.

- **Login:** `ssh bandit3@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 2)*

---

## 🧠 What I Learned

- How Linux hides files (dot-files)
- How to list hidden files with `ls -a` or `ls -la`
- The convention of using `.` prefix for configuration and hidden files

---

## 🛠 Commands Used

```bash
ls
cd inhere
ls -la
cat .hidden
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Explore the home directory**

```bash
ls
```

Output:
```
inhere
```

Navigate into it:

```bash
cd inhere
```

**Step 2 — List files (including hidden ones)**

```bash
ls
```

Output: *(nothing visible)*

The directory appears empty. But Linux hides files whose names begin with `.`:

```bash
ls -la
```

Output:
```
total 12
drwxr-xr-x 2 root    root    4096 ...
drwxr-xr-x 3 root    root    4096 ...
-rw-r----- 1 bandit4 bandit3   33 ... .hidden
```

**Step 3 — Read the hidden file**

```bash
cat .hidden
```

This outputs the password for Level 4.

---

## 🔐 Cybersecurity / SOC Relevance

Dot-files are extremely relevant in security investigations:

- **Malware persistence**: Attackers frequently drop backdoors, scripts, or SSH keys as hidden dot-files (e.g., `.bashrc` modifications, `.ssh/authorized_keys` additions)
- **Forensic analysis**: When imaging a compromised system, analysts must always include hidden files in their examination
- **Rootkit detection**: Some rootkits hide their files using the dot prefix or manipulate `ls` to omit them entirely

A SOC Analyst reviewing a compromised Linux host should always use `ls -la` — never just `ls`.

---

## 💡 Key Takeaways

- `ls -a` shows all files including hidden ones
- `ls -la` shows hidden files with full details (permissions, owner, size, date)
- Any file starting with `.` is hidden by default in most Linux file browsers and `ls`
- Configuration files (`.bashrc`, `.bash_history`, `.ssh/`) are dot-files — check them during investigations

---

## ❌ Common Mistakes

- Using `ls` without `-a` and assuming the directory is empty
- Forgetting that `.` and `..` are also shown by `ls -a` (they represent current and parent directories)
