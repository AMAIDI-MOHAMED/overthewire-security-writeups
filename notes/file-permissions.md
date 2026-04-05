# 🔒 File Permissions — Reference Notes

> Understanding Linux file permissions is critical for both system administration and cybersecurity work. Misconfigured permissions are one of the most common sources of security vulnerabilities.

---

## 📋 Reading Permission Output

When you run `ls -la`, each file is shown with a permission string like:

```
-rwxr-xr--  1  alice  staff  1234  Jan  1 12:00  script.sh
```

Breaking it down:

```
- rwx r-x r--
│ │   │   │
│ │   │   └── Other (everyone else): read only
│ │   └────── Group: read + execute
│ └────────── Owner (user): read + write + execute
└──────────── File type: - = file, d = directory, l = symlink
```

---

## 🔤 The Three Permission Types

| Symbol | Permission | For Files | For Directories |
|--------|-----------|-----------|-----------------|
| `r` | Read | View file contents | List directory contents (`ls`) |
| `w` | Write | Modify file | Create/delete files inside |
| `x` | Execute | Run as a program | Enter directory (`cd`) |
| `-` | None | No permission | No permission |

---

## 👥 The Three Permission Groups

| Group | Who it applies to |
|-------|-------------------|
| **User (u)** | The file's owner |
| **Group (g)** | Members of the file's group |
| **Other (o)** | Everyone else |

---

## 🔢 Octal (Numeric) Permissions

Each permission set can be represented as a 3-bit number:

| Binary | Octal | Meaning |
|--------|-------|---------|
| 000 | 0 | No permissions |
| 001 | 1 | Execute only |
| 010 | 2 | Write only |
| 011 | 3 | Write + Execute |
| 100 | 4 | Read only |
| 101 | 5 | Read + Execute |
| 110 | 6 | Read + Write |
| 111 | 7 | Read + Write + Execute |

### Common Permission Values

| Octal | Symbolic | Common Use Case |
|-------|---------|----------------|
| `755` | `rwxr-xr-x` | Executable scripts, directories |
| `644` | `rw-r--r--` | Regular files, config files |
| `600` | `rw-------` | Private files (SSH keys!) |
| `400` | `r--------` | Read-only private files |
| `777` | `rwxrwxrwx` | Writable by all (avoid!) |
| `000` | `----------` | No access at all |

---

## 🛠 Changing Permissions: `chmod`

### Using Symbolic Mode

```bash
chmod u+x script.sh      # Add execute for owner
chmod g-w file.txt       # Remove write for group
chmod o=r file.txt       # Set other to read-only exactly
chmod a+r file.txt       # Add read for all (a = all = ugo)
chmod u+x,g-w file.txt   # Multiple changes at once
```

### Using Numeric (Octal) Mode

```bash
chmod 755 script.sh      # rwxr-xr-x
chmod 644 config.txt     # rw-r--r--
chmod 600 ~/.ssh/id_rsa  # rw------- (required for SSH keys)
chmod 400 keyfile        # r-------- (read-only private)
chmod -R 755 directory/  # Apply recursively to directory
```

---

## 👤 Changing Ownership: `chown`

```bash
chown alice file.txt             # Change owner to alice
chown alice:developers file.txt  # Change owner and group
chown :developers file.txt       # Change group only
chown -R alice /home/alice/      # Recursive ownership change
```

### `chgrp` — Change Group Only

```bash
chgrp developers file.txt
```

---

## ⚠️ Special Permission Bits

### SUID (Set User ID) — `4xxx`

When set on an executable, it runs with the **file owner's** privileges, not the caller's.

```bash
chmod u+s executable    # Symbolic
chmod 4755 executable   # Numeric (rwsr-xr-x)
```

Example: `/usr/bin/passwd` has SUID so regular users can change their password (runs as root).

**Security risk:** SUID root files are prime targets for privilege escalation. Attackers look for writable SUID binaries.

### SGID (Set Group ID) — `2xxx`

On executables: runs with the group's privileges.
On directories: new files inherit the directory's group.

```bash
chmod g+s directory/    # Symbolic
chmod 2755 directory/   # Numeric (rwxr-sr-x)
```

### Sticky Bit — `1xxx`

On directories: only the file owner can delete their own files (even if others have write permission).

```bash
chmod +t /tmp           # Symbolic
chmod 1777 /tmp         # Numeric (rwxrwxrwt)
```

`/tmp` always has the sticky bit — that's why you can write to `/tmp` but not delete others' files.

---

## 🔐 Security Implications for SOC Analysts

### Finding SUID/SGID Files (Privilege Escalation)

```bash
find / -perm -4000 -type f 2>/dev/null    # Find all SUID files
find / -perm -2000 -type f 2>/dev/null    # Find all SGID files
find / -perm -6000 -type f 2>/dev/null    # Find both
```

### Finding World-Writable Files (Misconfiguration)

```bash
find / -perm -o+w -type f 2>/dev/null     # World-writable files
find / -perm -o+w -type d 2>/dev/null     # World-writable directories
```

### Checking SSH Key Permissions

SSH will refuse to use a key if it's too permissive:
```bash
chmod 600 ~/.ssh/id_rsa         # Correct permission
chmod 700 ~/.ssh/               # Correct directory permission
```

---

## 📖 Quick Reference

```bash
ls -la                       # View permissions
chmod 755 file               # Set permissions (numeric)
chmod u+x file               # Add execute for owner (symbolic)
chown user:group file        # Change owner and group
find / -perm -4000 2>/dev/null  # Find SUID files
stat file                    # Detailed file metadata including permissions
```

---

> 💡 **Remember:** "640" means owner can read/write, group can read, others have no access. When in doubt, use the principle of least privilege — only grant the permissions that are actually needed.
