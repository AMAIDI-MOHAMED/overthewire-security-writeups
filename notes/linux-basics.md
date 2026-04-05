# 🐧 Linux Basics — Reference Notes

> A beginner-friendly reference for the core Linux commands used in cybersecurity work and the OverTheWire Bandit wargame.

---

## 📂 Navigating the Filesystem

The Linux filesystem is organized as a tree, starting from `/` (root). Here are the essential navigation commands:

### `pwd` — Print Working Directory

Shows your current location in the filesystem.

```bash
pwd
# Output: /home/bandit0
```

### `ls` — List Directory Contents

```bash
ls              # List visible files and directories
ls -a           # List ALL files including hidden (dot files)
ls -l           # Long format: shows permissions, size, owner, date
ls -la          # Long format + hidden files (most useful)
ls -lh          # Human-readable sizes (e.g., 4.0K instead of 4096)
ls /etc         # List contents of a specific path
```

### `cd` — Change Directory

```bash
cd /home/user       # Navigate to an absolute path
cd Documents        # Navigate to a relative path
cd ..               # Go up one directory (parent)
cd ~                # Go to your home directory
cd -                # Go back to the previous directory
```

### `mkdir` — Make Directory

```bash
mkdir myfolder               # Create a directory
mkdir -p path/to/deep/dir    # Create nested directories
```

---

## 📄 Working with Files

### `cat` — Concatenate and Print Files

```bash
cat file.txt            # Print entire file to terminal
cat file1 file2         # Print multiple files
cat -n file.txt         # Show line numbers
```

### `less` — View Files Page by Page

Better than `cat` for large files:

```bash
less largefile.txt
```

| Key | Action |
|-----|--------|
| `Space` / `f` | Next page |
| `b` | Previous page |
| `/pattern` | Search forward |
| `n` | Next search result |
| `q` | Quit |

### `head` and `tail` — View File Beginnings/Endings

```bash
head file.txt        # Show first 10 lines
head -n 20 file.txt  # Show first 20 lines
tail file.txt        # Show last 10 lines
tail -f log.txt      # Follow a file in real time (great for logs)
```

### `cp`, `mv`, `rm` — Copy, Move, Delete

```bash
cp source.txt dest.txt          # Copy a file
cp -r source_dir dest_dir       # Copy a directory recursively
mv oldname.txt newname.txt      # Rename a file
mv file.txt /tmp/               # Move a file to /tmp
rm file.txt                     # Delete a file (no undo!)
rm -rf directory/               # Delete a directory recursively (use with caution)
```

### `touch` — Create Empty Files / Update Timestamps

```bash
touch newfile.txt    # Create empty file or update timestamp
```

---

## 🔍 Searching

### `grep` — Search File Contents

```bash
grep "pattern" file.txt           # Case-sensitive search
grep -i "pattern" file.txt        # Case-insensitive
grep -r "pattern" /directory/     # Recursive search in directory
grep -n "pattern" file.txt        # Show line numbers
grep -v "pattern" file.txt        # Invert match (lines NOT matching)
grep -c "pattern" file.txt        # Count matching lines
```

### `find` — Find Files on the Filesystem

```bash
find /path -name "file.txt"            # Find by name
find / -user bandit7 2>/dev/null       # Find by owner
find . -type f -size +1M               # Files larger than 1MB
find . -mmin -60                       # Modified in last 60 minutes
find / -perm -4000 2>/dev/null         # Find SUID files
```

---

## ⌨️ Input/Output Redirection

### Standard Streams

Every Linux process has three streams:
- **stdin (0)** — standard input (keyboard by default)
- **stdout (1)** — standard output (terminal by default)
- **stderr (2)** — error output (terminal by default)

### Redirection Operators

```bash
command > file.txt      # Redirect stdout to file (overwrite)
command >> file.txt     # Redirect stdout to file (append)
command 2>/dev/null     # Discard error messages
command 2>&1            # Redirect stderr to same place as stdout
command < file.txt      # Feed file as stdin to command
```

### Pipes

```bash
command1 | command2    # Send stdout of command1 to stdin of command2
ls -la | grep "^-"     # List only regular files
cat file | sort | uniq # Sort and deduplicate
```

---

## 📌 Useful Miscellaneous Commands

```bash
echo "text"          # Print text to terminal
echo $HOME           # Print an environment variable
man ls               # Open the manual page for a command
which python3        # Find where a command is located
whoami               # Show current username
id                   # Show current user and group IDs
history              # Show command history
clear                # Clear the terminal screen
exit                 # Close the current shell session
```

---

## 🗂️ Important Directories

| Path | Description |
|------|-------------|
| `/` | Root of the filesystem |
| `/home` | User home directories |
| `/etc` | System configuration files |
| `/var/log` | Log files |
| `/tmp` | Temporary files (writable by all) |
| `/bin`, `/usr/bin` | Executable programs |
| `/proc` | Virtual filesystem for process info |
| `/dev` | Device files |

---

> 💡 **Pro tip:** When you're unsure about a command, run `man command` to read its manual page. For a quick summary, try `command --help`.
