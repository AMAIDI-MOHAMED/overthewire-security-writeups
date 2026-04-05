# ⚙️ Useful Commands — Quick Reference

> A curated cheat sheet of the most important commands for OverTheWire Bandit and general cybersecurity work.

---

## 📂 File Operations

```bash
# Read files
cat file.txt                    # print entire file
less file.txt                   # pager (q to quit)
head -n 20 file.txt             # first 20 lines
tail -n 20 file.txt             # last 20 lines
tail -f /var/log/syslog         # follow in real-time

# File type detection
file mystery_file               # detect file type from magic bytes
file ./*                        # detect types of all files in directory

# Count lines, words, characters
wc -l file.txt                  # line count
wc -w file.txt                  # word count
wc -c file.txt                  # byte count
```

---

## 🔍 Searching

```bash
# Search content within files
grep "pattern" file.txt         # basic search
grep -i "pattern" file.txt      # case-insensitive
grep -r "pattern" /etc/         # recursive directory search
grep -n "pattern" file.txt      # show line numbers
grep -v "pattern" file.txt      # invert match (exclude pattern)
grep -E "regex" file.txt        # extended regex
grep -l "pattern" *.txt         # only show filenames with matches
grep -c "pattern" file.txt      # count matching lines

# Search for files
find / -name "filename"         # by name
find / -type f -size +1M        # files > 1MB
find / -user root -perm /4000   # SUID files owned by root
find / -mtime -1                # modified in last 24h
find / -newer reference_file    # newer than a specific file
find . -name "*.log" 2>/dev/null

# Extract readable text from binary files
strings binary_file
strings binary_file | grep "http"
```

---

## 📊 Text Processing

```bash
# Sort lines
sort file.txt                   # alphabetical
sort -n file.txt                # numerical
sort -r file.txt                # reverse order
sort -k2 file.txt               # sort by 2nd field

# Find unique/duplicate lines (requires sorted input)
sort file.txt | uniq            # remove duplicates
sort file.txt | uniq -u         # unique lines only (appear once)
sort file.txt | uniq -d         # duplicate lines only
sort file.txt | uniq -c         # count occurrences

# Cut columns from delimited files
cut -d':' -f1 /etc/passwd       # extract 1st field (colon-delimited)
cut -d',' -f2,4 data.csv        # extract fields 2 and 4

# Replace/translate characters
tr 'a-z' 'A-Z' < file.txt       # uppercase
tr -d '\n' < file.txt           # remove newlines
tr -s ' ' < file.txt            # squeeze repeated spaces

# Stream editor (find and replace)
sed 's/old/new/g' file.txt      # replace all occurrences
sed -n '5,10p' file.txt         # print lines 5-10
sed '/pattern/d' file.txt       # delete lines matching pattern

# Field/column processing
awk '{print $1}' file.txt       # print 1st field
awk -F':' '{print $1,$3}' /etc/passwd   # custom delimiter
awk '{sum += $2} END {print sum}' data  # sum a column
```

---

## 🔐 Encoding & Decoding

```bash
# Base64
base64 file.txt                 # encode
base64 -d encoded.txt           # decode
echo "hello" | base64           # encode string
echo "aGVsbG8=" | base64 -d    # decode string

# ROT13
echo "Hello" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

# Hex dump
xxd file.bin                    # hex + ASCII view
xxd -r hex_file.txt             # reverse: hex to binary
od -c file.bin                  # octal dump with characters

# MD5/SHA hashing
md5sum file.txt
sha1sum file.txt
sha256sum file.txt
echo -n "text" | sha256sum      # hash a string
```

---

## 🌐 Network Commands

```bash
# SSH
ssh user@host -p 2220
ssh -i ~/.ssh/key user@host

# Netcat
nc host port                    # connect to service
nc -lvnp 4444                   # listen on port 4444

# OpenSSL
openssl s_client -connect host:443
openssl s_client -connect host:port

# Download files
wget http://example.com/file.txt
curl -O http://example.com/file.txt
curl -s http://example.com/api   # silent mode

# Port/connection info
ss -tulnp                        # listening ports
netstat -tulnp                   # older alternative
nmap -p- host                    # scan all ports
```

---

## ⚙️ Process & System

```bash
ps aux                           # all running processes
ps aux | grep "suspicious"       # find a specific process
kill -9 <PID>                    # force kill a process
top                              # real-time process view
htop                             # improved top (if available)

# User and privilege info
whoami
id
sudo -l                          # list allowed sudo commands
cat /etc/passwd                  # list users
cat /etc/group                   # list groups

# Environment
env                              # all environment variables
echo $PATH
echo $HOME
export MY_VAR="value"
```

---

## 🛠 Compression & Archives

```bash
# tar archives
tar -cvf archive.tar files/     # create
tar -xvf archive.tar            # extract
tar -czvf archive.tar.gz files/ # create with gzip compression
tar -xzvf archive.tar.gz        # extract gzip

# gzip / bzip2 / xz
gzip file.txt                   # compress → file.txt.gz
gunzip file.txt.gz              # decompress
bzip2 file.txt                  # compress → file.txt.bz2
bunzip2 file.txt.bz2            # decompress
xz file.txt                     # compress → file.txt.xz
unxz file.txt.xz                # decompress

# zip
zip archive.zip files/
unzip archive.zip
```

---

## 💡 Shell Productivity Tips

```bash
# History
history                          # view command history
history | grep "ssh"             # search history
!!                               # repeat last command
!grep                            # repeat last grep command

# Job control
command &                        # run in background
Ctrl+Z                           # pause current job
bg                               # resume in background
fg                               # bring to foreground
jobs                             # list background jobs

# Useful shortcuts
Ctrl+C    # kill current process
Ctrl+D    # exit shell / end input
Ctrl+L    # clear screen
Ctrl+R    # reverse search history
Ctrl+A    # jump to start of line
Ctrl+E    # jump to end of line
Tab       # autocomplete
```
