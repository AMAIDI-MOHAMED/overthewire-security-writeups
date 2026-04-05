# 🐧 Linux Basics — Reference Notes

> Beginner-friendly reference for core Linux commands used in cybersecurity and daily administration.

---

## 📁 Navigation

| Command | Description | Example |
|---|---|---|
| `pwd` | Print working directory (where you are) | `pwd` → `/home/user` |
| `ls` | List files and directories | `ls -la` (all, detailed) |
| `cd` | Change directory | `cd /var/log` |
| `cd ..` | Go up one directory | `cd ../..` (up two levels) |
| `cd ~` | Go to home directory | `cd ~` |
| `cd -` | Go to previous directory | `cd -` |

---

## 📄 Reading Files

| Command | Description | Example |
|---|---|---|
| `cat` | Print entire file to terminal | `cat /etc/passwd` |
| `less` | Page through a file (press `q` to quit) | `less bigfile.log` |
| `more` | Similar to `less` (older, less features) | `more file.txt` |
| `head` | Show first N lines (default: 10) | `head -20 file.txt` |
| `tail` | Show last N lines (default: 10) | `tail -f /var/log/syslog` |

> 💡 **Tip**: `tail -f` follows a file in real-time — extremely useful for watching live logs during an incident.

---

## 📝 Creating & Editing Files

| Command | Description |
|---|---|
| `touch filename` | Create an empty file |
| `nano filename` | Open file in nano text editor |
| `vim filename` | Open file in vim (steeper learning curve) |
| `echo "text" > file` | Write text to a file (overwrites) |
| `echo "text" >> file` | Append text to a file |

---

## 📋 Copying, Moving & Deleting

| Command | Description | Example |
|---|---|---|
| `cp` | Copy file or directory | `cp file.txt backup.txt` |
| `mv` | Move or rename | `mv old.txt new.txt` |
| `rm` | Remove file | `rm file.txt` |
| `rm -r` | Remove directory recursively | `rm -r folder/` |
| `mkdir` | Create directory | `mkdir logs` |
| `rmdir` | Remove empty directory | `rmdir empty_folder` |

> ⚠️ **Warning**: There is no trash/recycle bin in Linux CLI. `rm` is permanent.

---

## 🔗 Pipes and Redirection

Pipes (`|`) and redirection (`>`, `>>`, `<`) are foundational Linux skills.

```bash
# Pipe: send output of one command as input to another
ls -la | grep ".log"

# Redirect output to a file (overwrite)
echo "hello" > output.txt

# Redirect output to a file (append)
echo "world" >> output.txt

# Redirect errors to /dev/null (discard them)
find / -name "*.conf" 2>/dev/null

# Redirect both stdout and stderr
command > output.txt 2>&1
```

---

## 🔍 Searching

```bash
# Search for text inside files
grep "error" /var/log/syslog
grep -r "password" /etc/          # recursive
grep -i "failed" auth.log         # case-insensitive
grep -n "root" /etc/passwd        # show line numbers
grep -v "INFO" application.log    # show lines NOT matching

# Find files and directories
find /home -name "*.txt"
find / -type f -size +1M          # files larger than 1MB
find / -mtime -1                  # modified in the last 24 hours
```

---

## 📊 System Information

```bash
whoami          # current username
id              # user ID, group ID, and group memberships
hostname        # machine hostname
uname -a        # kernel version and OS info
uptime          # how long the system has been running
df -h           # disk space usage (human-readable)
du -sh folder/  # size of a specific directory
ps aux          # list running processes
top / htop      # real-time process monitor
```

---

## 💡 Useful Tips

- Use `Tab` for autocompletion — saves time and avoids typos
- Use `Ctrl+C` to stop a running command
- Use `Ctrl+D` to exit a shell or end input
- Use `history` to see previous commands
- Use `man <command>` to read the manual for any command (e.g., `man grep`)
- Use `--help` for a quick summary (e.g., `ls --help`)
