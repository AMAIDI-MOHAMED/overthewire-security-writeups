# 🔐 Bandit Level 5

## 🎯 Goal

Find the password inside the `inhere` directory. The file has these properties:
- Human-readable
- 1033 bytes in size
- Not executable

- **Login:** `ssh bandit5@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 4)*

---

## 🧠 What I Learned

- How to use `find` with multiple conditions
- How to filter files by size, type, and permissions
- Combining `find` output with other commands

---

## 🛠 Commands Used

```bash
find ./inhere -type f -size 1033c ! -executable
```

Then read the result:

```bash
cat ./inhere/maybehere07/.file2
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Explore the directory**

```bash
ls inhere/
```

There are many subdirectories, each containing several files. Manually searching is impractical.

**Step 2 — Use `find` with multiple filters**

```bash
find ./inhere -type f -size 1033c ! -executable
```

Breaking this down:
- `find ./inhere` — search inside the `inhere` directory
- `-type f` — only files (not directories)
- `-size 1033c` — exactly 1033 bytes (`c` = bytes)
- `! -executable` — NOT executable (the `!` negates the condition)

Output:
```
./inhere/maybehere07/.file2
```

**Step 3 — Read the file**

```bash
cat ./inhere/maybehere07/.file2
```

This outputs the password for Level 6.

---

## 🔐 Cybersecurity / SOC Relevance

`find` is one of the most powerful tools in a security analyst's arsenal:

- **Incident response**: Quickly locate recently modified files (`-newer`), files with SUID bits (`-perm /4000`), or world-writable files (`-perm -o+w`)
- **Threat hunting**: Search for files with suspicious sizes, unusual owners, or specific extensions across an entire filesystem
- **Forensics**: Identify files created within a specific time window during a breach

A SOC Analyst able to use `find` efficiently can dramatically speed up investigation work on compromised Linux systems.

---

## 💡 Key Takeaways

- `find` supports combining multiple conditions: `-type`, `-size`, `-perm`, `-user`, `-newer`, and more
- Size suffix matters: `c` = bytes, `k` = kilobytes, `M` = megabytes
- `!` negates a condition — very useful for exclusions
- Always pipe `find` output to `2>/dev/null` in real scenarios to suppress permission-denied errors

---

## ❌ Common Mistakes

- Using `-size 1033` without the `c` suffix (defaults to 512-byte blocks, not bytes)
- Forgetting `!` before `-executable` to negate the condition
- Not using `-type f` and accidentally matching directories
