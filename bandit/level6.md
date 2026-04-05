# Bandit Level 6 → Level 7

## 🎯 Goal

The password for the next level is stored somewhere on the server and has all of the following properties:
- Owned by user `bandit7`
- Owned by group `bandit6`
- 33 bytes in size

**Connect with:** `ssh bandit6@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- Searching the **entire filesystem** with `find /`
- Filtering by file ownership (`-user`, `-group`)
- Suppressing "Permission denied" noise by redirecting stderr to `/dev/null`
- The importance of stderr vs. stdout redirection

---

## 🛠 Commands Used

```bash
# Search the entire server for the file matching all criteria
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null

# Read the found file
cat /var/lib/dpkg/info/bandit7.password
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Connect and assess**

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

The home directory is empty. The hint says "stored somewhere on the server" — meaning we need to search the whole filesystem, not just the home directory.

**Step 2 — Search the entire filesystem**

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

Breaking it down:
- `/` — start from the root of the filesystem (everything)
- `-type f` — only regular files
- `-user bandit7` — the file must be owned by the user `bandit7`
- `-group bandit6` — the file must belong to the group `bandit6`
- `-size 33c` — exactly 33 bytes
- `2>/dev/null` — redirect **standard error** (fd 2) to `/dev/null` (discard it)

Without `2>/dev/null`, the output is flooded with "Permission denied" messages for directories we can't read. Sending stderr to `/dev/null` silences those errors so we can see the actual result.

**Step 3 — Read the result**

The command returns one path:
```
/var/lib/dpkg/info/bandit7.password
```

```bash
cat /var/lib/dpkg/info/bandit7.password
```

---

## 🔐 Cybersecurity / SOC Relevance

The technique of redirecting stderr (`2>/dev/null`) is extremely common in security scripting and enumeration. Attackers and defenders alike use it to clean up noisy output during filesystem searches. In penetration testing and red-team operations, `find / -perm -4000 2>/dev/null` is a go-to command for finding SUID files that could be used for privilege escalation.

For SOC Analysts performing forensic triage:
- Searching by file ownership helps identify files dropped by compromised service accounts
- `find / -user www-data -newer /etc/passwd 2>/dev/null` finds files recently created by a web server user — a common sign of web shell deployment

---

## 💡 Key Takeaways

- `find /` searches the entire filesystem — use it when the file's location is unknown
- `-user` and `-group` filter by ownership, not just permissions
- `2>/dev/null` suppresses error messages — `2>` redirects file descriptor 2 (stderr)
- `/dev/null` is the "black hole" of Linux — anything written there is discarded

---

## ❌ Common Mistakes

- **Forgetting `2>/dev/null`** — your terminal gets flooded with "Permission denied" lines, hiding the real result
- **Using `1>/dev/null` instead** — that discards stdout (the output you actually want!); use `2>` for errors
- **Starting search from home directory (`~`)** — the file is not in the home directory; start from `/`
