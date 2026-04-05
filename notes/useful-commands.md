# 🛠️ Useful Commands — Quick Reference

> A consolidated cheat sheet of the most important Linux commands for the Bandit wargame and real-world cybersecurity work.

---

## 📁 File Navigation

| Command | Description | Example |
|---------|-------------|---------|
| `pwd` | Print current directory | `pwd` |
| `ls -la` | List all files with details | `ls -la /home` |
| `cd` | Change directory | `cd /tmp` |
| `cd ..` | Go up one level | `cd ..` |
| `cd ~` | Go to home directory | `cd ~` |
| `file` | Identify file type by content | `file mystery.bin` |
| `stat` | Detailed file metadata | `stat file.txt` |

---

## 📄 Reading Files

| Command | Description | Example |
|---------|-------------|---------|
| `cat` | Print file contents | `cat readme.txt` |
| `less` | Paginate through file | `less bigfile.log` |
| `head -n 20` | First 20 lines | `head -n 20 log.txt` |
| `tail -n 20` | Last 20 lines | `tail -n 20 log.txt` |
| `tail -f` | Follow file in real time | `tail -f /var/log/syslog` |
| `wc -l` | Count lines | `wc -l data.txt` |
| `strings` | Extract readable text from binary | `strings binary.bin` |

---

## 🔍 Searching

| Command | Description | Example |
|---------|-------------|---------|
| `grep "pattern" file` | Search file for pattern | `grep "error" log.txt` |
| `grep -i` | Case-insensitive search | `grep -i "fail" auth.log` |
| `grep -r` | Recursive search | `grep -r "password" /etc/` |
| `grep -v` | Invert match | `grep -v "^#" config.txt` |
| `grep -n` | Show line numbers | `grep -n "root" /etc/passwd` |
| `find . -name` | Find by filename | `find / -name "*.log"` |
| `find . -type f` | Find regular files only | `find . -type f` |
| `find . -size` | Find by size | `find . -size +1M` |
| `find . -user` | Find by owner | `find / -user bandit7` |
| `find . -mmin -60` | Modified in last 60 minutes | `find / -mmin -60` |

---

## 📊 Text Processing

| Command | Description | Example |
|---------|-------------|---------|
| `sort` | Sort lines alphabetically | `sort data.txt` |
| `sort -n` | Sort numerically | `sort -n numbers.txt` |
| `sort -r` | Reverse sort | `sort -r data.txt` |
| `uniq` | Remove consecutive duplicates | `sort file \| uniq` |
| `uniq -u` | Print unique-only lines | `sort file \| uniq -u` |
| `uniq -c` | Count occurrences | `sort file \| uniq -c` |
| `cut -d: -f1` | Cut field from delimited text | `cut -d: -f1 /etc/passwd` |
| `awk '{print $1}'` | Print first field | `awk '{print $1}' access.log` |
| `sed 's/old/new/'` | String substitution | `sed 's/foo/bar/' file.txt` |
| `tr 'a-z' 'A-Z'` | Translate characters | `echo "hello" \| tr 'a-z' 'A-Z'` |
| `wc -l` | Count lines | `wc -l file.txt` |

---

## 🔐 Encoding & Decoding

| Command | Description | Example |
|---------|-------------|---------|
| `base64 -d` | Decode Base64 | `base64 -d encoded.txt` |
| `base64` | Encode to Base64 | `echo "hello" \| base64` |
| `xxd` | Hex dump | `xxd binary.bin` |
| `xxd -r` | Reverse hex dump to binary | `xxd -r hexdump.txt > output.bin` |
| `tr 'A-Za-z' 'N-ZA-Mn-za-m'` | ROT13 decode | `cat file \| tr 'A-Za-z' 'N-ZA-Mn-za-m'` |

---

## 📦 Compression & Archives

| Command | Description | Example |
|---------|-------------|---------|
| `gunzip file.gz` | Decompress gzip | `gunzip archive.gz` |
| `gzip file` | Compress to gzip | `gzip data.txt` |
| `bunzip2 file.bz2` | Decompress bzip2 | `bunzip2 archive.bz2` |
| `bzip2 file` | Compress to bzip2 | `bzip2 data.txt` |
| `tar xf file.tar` | Extract tar archive | `tar xf archive.tar` |
| `tar czf out.tar.gz dir/` | Create gzipped tar | `tar czf backup.tar.gz /home/user` |
| `tar tf file.tar` | List tar contents | `tar tf archive.tar` |

---

## 🌐 Networking

| Command | Description | Example |
|---------|-------------|---------|
| `ssh user@host -p port` | Connect via SSH | `ssh bandit0@bandit.labs.overthewire.org -p 2220` |
| `ssh -i key user@host` | SSH with key file | `ssh -i sshkey.private bandit14@localhost -p 2220` |
| `nc host port` | TCP connection (netcat) | `nc localhost 30000` |
| `openssl s_client -connect h:p` | TLS connection | `openssl s_client -connect localhost:30001 -quiet` |
| `ss -tulnp` | Show listening ports | `ss -tulnp` |
| `curl http://url` | HTTP request | `curl http://example.com` |

---

## 📋 File Permissions

| Command | Description | Example |
|---------|-------------|---------|
| `chmod 755 file` | Set permissions (octal) | `chmod 755 script.sh` |
| `chmod u+x file` | Add execute for owner | `chmod u+x run.sh` |
| `chmod -R 644 dir/` | Recursive permission change | `chmod -R 644 /var/www/html/` |
| `chown user:group file` | Change owner and group | `chown root:root /etc/hosts` |
| `ls -la` | View permissions | `ls -la /home` |
| `find / -perm -4000` | Find SUID files | `find / -perm -4000 2>/dev/null` |

---

## ⚙️ System & Process

| Command | Description | Example |
|---------|-------------|---------|
| `whoami` | Current username | `whoami` |
| `id` | User ID and group memberships | `id` |
| `ps aux` | List all running processes | `ps aux` |
| `ps aux \| grep nginx` | Find specific process | `ps aux \| grep nginx` |
| `kill PID` | Kill a process | `kill 1234` |
| `history` | Command history | `history \| grep ssh` |
| `env` | Show environment variables | `env` |
| `which command` | Locate a command binary | `which python3` |
| `man command` | Read the manual | `man grep` |
| `command --help` | Quick help | `grep --help` |

---

## 🚨 I/O Redirection Quick Reference

```bash
command > file          # Redirect stdout to file (overwrite)
command >> file         # Append stdout to file
command 2>/dev/null     # Discard errors
command 2>&1 | less     # View both stdout and stderr together
command < file          # Use file as stdin
command1 | command2     # Pipe: stdout of 1 → stdin of 2
```

---

## 💡 One-Liners for Log Analysis

```bash
# Count unique IPs in an access log
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -20

# Find most common HTTP status codes
awk '{print $9}' access.log | sort | uniq -c | sort -rn

# Find failed SSH login attempts
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn

# Find all SUID files (privilege escalation check)
find / -perm -4000 -type f 2>/dev/null

# Find files modified in the last hour
find / -mmin -60 -type f 2>/dev/null

# Check open network connections
ss -tulnp

# Extract strings with surrounding context from a binary
strings -n 8 binary | grep -i "password\|key\|token\|secret"
```

---

> 💡 **Remember:** Mastering these commands isn't about memorization — it's about knowing what's possible so you can look up the exact syntax when needed. The more you use them, the more naturally they come.
