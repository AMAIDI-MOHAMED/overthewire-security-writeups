# 🔒 Linux File Permissions — Reference Notes

> Understanding file permissions is critical for security. Misconfigured permissions are one of the most common causes of privilege escalation and unauthorized access.

---

## 🧩 Understanding the Permission String

When you run `ls -la`, you see output like this:

```
-rwxr-xr--  1  alice  staff  4096  Apr 1  script.sh
drwxr-xr-x  2  root   root   4096  Apr 1  config/
```

The first column is the **permission string**. Let's break it down:

```
- rwx r-x r--
│ │   │   └── Other (everyone else): read only
│ │   └────── Group: read + execute
│ └────────── Owner: read + write + execute
└──────────── File type: - = file, d = directory, l = symlink
```

---

## 🔤 Permission Characters

| Symbol | Meaning |
|---|---|
| `r` | Read — view file contents or list directory |
| `w` | Write — modify file or create/delete files in directory |
| `x` | Execute — run file as program or enter directory |
| `-` | Permission not granted |

---

## 👥 Permission Groups

| Group | Who |
|---|---|
| **Owner (u)** | The user who owns the file |
| **Group (g)** | Members of the file's group |
| **Other (o)** | Everyone else on the system |

---

## 🔢 Numeric (Octal) Notation

Permissions are also represented as numbers:

| Permission | Binary | Octal |
|---|---|---|
| `---` | 000 | 0 |
| `--x` | 001 | 1 |
| `-w-` | 010 | 2 |
| `-wx` | 011 | 3 |
| `r--` | 100 | 4 |
| `r-x` | 101 | 5 |
| `rw-` | 110 | 6 |
| `rwx` | 111 | 7 |

**Common permission patterns:**

| Octal | Symbolic | Typical use |
|---|---|---|
| `644` | `rw-r--r--` | Normal files |
| `755` | `rwxr-xr-x` | Directories, scripts |
| `600` | `rw-------` | Private files (SSH keys) |
| `700` | `rwx------` | Private directories |
| `777` | `rwxrwxrwx` | ⚠️ Dangerous — avoid in production |

---

## 🔧 Changing Permissions with `chmod`

```bash
# Using symbolic notation
chmod u+x script.sh       # add execute for owner
chmod g-w file.txt        # remove write for group
chmod o=r file.txt        # set other to read-only
chmod a+r file.txt        # add read for all (a = all)

# Using octal notation
chmod 644 file.txt        # rw-r--r--
chmod 755 script.sh       # rwxr-xr-x
chmod 600 id_rsa          # rw------- (SSH private key)
chmod -R 755 directory/   # apply recursively
```

---

## 🔧 Changing Ownership with `chown`

```bash
# Change owner
chown alice file.txt

# Change owner and group
chown alice:developers file.txt

# Change only group
chgrp developers file.txt

# Recursive change
chown -R www-data:www-data /var/www/html/
```

---

## ⚠️ Special Permission Bits

### SUID (Set User ID) — `s` in owner execute position
```
-rwsr-xr-x  root  /usr/bin/passwd
```
When a file with SUID is executed, it runs with the **owner's** privileges (not the caller's). Common in tools like `passwd` that need root access. **Common attack vector** — attackers look for SUID binaries they can exploit.

### SGID (Set Group ID) — `s` in group execute position
```
-rwxr-sr-x  staff  /usr/bin/write
```
When executed, the process runs with the **group's** privileges.

### Sticky Bit — `t` in other execute position
```
drwxrwxrwt  root  /tmp
```
In directories, prevents users from deleting files owned by others, even if they have write access. Used on `/tmp`.

```bash
# Find all SUID binaries on the system
find / -perm /4000 2>/dev/null

# Find world-writable files (security risk)
find / -perm -o+w -type f 2>/dev/null
```

---

## 🔐 SOC Analyst Relevance

| Scenario | What to Look For |
|---|---|
| Privilege escalation | SUID/SGID binaries owned by root |
| Misconfigured servers | World-writable files (`777`) or directories |
| SSH key exposure | `id_rsa` files with `644` instead of `600` |
| Web shell detection | PHP/script files in web root with execute bit |
| Backdoor persistence | Unexpected SUID binaries in `/tmp` or user homes |
