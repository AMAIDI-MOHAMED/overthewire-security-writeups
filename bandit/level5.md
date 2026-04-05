# Bandit Level 5 → Level 6

## 🎯 Goal

The password is stored in a file somewhere under the `inhere` directory and has all of the following properties:
- Human-readable
- 1033 bytes in size
- Not executable

**Connect with:** `ssh bandit5@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How to use `find` with multiple filter flags to locate a specific file
- Filtering by file size, type, and permissions
- The power of `find` for narrowing down suspects in a large directory tree

---

## 🛠 Commands Used

```bash
# Navigate to the directory containing the files
cd inhere

# Find the file matching all three criteria
find . -type f -size 1033c ! -executable

# Read the found file
cat ./maybehere07/.file2
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Survey the directory**

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
cd inhere
ls
```

There are many subdirectories (`maybehere00` through `maybehere19`), each containing multiple files. Searching manually is impractical.

**Step 2 — Build the `find` command**

`find` is one of the most powerful Linux commands for locating files. We can stack multiple conditions:

```bash
find . -type f -size 1033c ! -executable
```

Breaking it down:
- `.` — search starting from the current directory (recursively)
- `-type f` — only match regular files (not directories or links)
- `-size 1033c` — exactly 1033 bytes (`c` = bytes; `k` = kilobytes; `M` = megabytes)
- `! -executable` — the `!` means NOT; exclude files that are executable

**Step 3 — Examine the result**

The command returns one file, for example:
```
./maybehere07/.file2
```

You can verify it's human-readable with `file`:
```bash
file ./maybehere07/.file2
```

Output:
```
./maybehere07/.file2: ASCII text, with very long lines
```

**Step 4 — Read the password**

```bash
cat ./maybehere07/.file2
```

---

## 🔐 Cybersecurity / SOC Relevance

The `find` command is indispensable in incident response. SOC Analysts use it to:
- Locate recently modified files: `find / -mmin -60` (modified in last 60 minutes)
- Find files owned by a specific user: `find / -user www-data`
- Identify SUID/SGID files (privilege escalation risks): `find / -perm -4000`
- Hunt for large hidden files that might be exfiltrated data or logs

Mastering `find` with multiple criteria is a core forensic skill that separates effective incident responders from those who get lost in complex directory trees.

---

## 💡 Key Takeaways

- `find` is recursive by default — it searches all subdirectories
- `-size 1033c` means exactly 1033 bytes; `c` is the unit for bytes
- `! -executable` uses logical NOT to exclude files with execute permission
- Combining multiple `-type`, `-size`, and permission flags is how you pinpoint a file quickly

---

## ❌ Common Mistakes

- **Using wrong size units** — `-size 1033` without `c` means 1033 512-byte blocks, not bytes!
- **Forgetting `-type f`** — without it, `find` may match directories too
- **Searching from the wrong directory** — make sure you `cd inhere` first, or use the full path
