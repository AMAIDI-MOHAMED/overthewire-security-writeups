# 🔐 Bandit Level 6

## 🎯 Goal

The password is stored *somewhere on the server* with these properties:
- Owned by user `bandit7`
- Owned by group `bandit6`
- 33 bytes in size

- **Login:** `ssh bandit6@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 5)*

---

## 🧠 What I Learned

- How to search the entire filesystem with `find /`
- How to filter by file owner (`-user`) and group (`-group`)
- How to suppress "permission denied" errors in `find` output

---

## 🛠 Commands Used

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Search the entire filesystem**

Unlike previous levels, the file could be anywhere. We start from `/` (the root):

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

Breaking it down:
- `find /` — search from the filesystem root
- `-type f` — files only
- `-user bandit7` — owned by user `bandit7`
- `-group bandit6` — owned by group `bandit6`
- `-size 33c` — exactly 33 bytes
- `2>/dev/null` — redirect error output (permission denied messages) to `/dev/null` so they don't clutter results

Output:
```
/var/lib/dpkg/info/bandit7.password
```

**Step 2 — Read the file**

```bash
cat /var/lib/dpkg/info/bandit7.password
```

This outputs the password for Level 7.

---

## 🔐 Cybersecurity / SOC Relevance

Searching the filesystem by owner, group, and size is a key forensic technique:

- **Privilege escalation hunting**: Find files owned by root but writable by others (`-user root -perm -o+w`)
- **SUID binary discovery**: `find / -perm /4000 2>/dev/null` lists all SUID binaries — a common attack surface
- **Data exfiltration investigation**: Identify recently created files of suspicious sizes in tmp or home directories

Redirecting stderr (`2>/dev/null`) is a best practice when running `find` as a non-root user — otherwise, hundreds of "Permission denied" messages bury the actual results.

---

## 💡 Key Takeaways

- `2>/dev/null` silences error messages — essential for cleaner output during investigations
- `-user` and `-group` filter by ownership — powerful for privilege and access analysis
- Searching from `/` covers the entire filesystem including `/var`, `/tmp`, `/etc`, etc.

---

## ❌ Common Mistakes

- Starting the search in the home directory instead of `/` — the file could be anywhere
- Omitting `2>/dev/null` — "Permission denied" spam makes the real result hard to spot
- Confusing `-user` (username) with `-uid` (numeric user ID)
