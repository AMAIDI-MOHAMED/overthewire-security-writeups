# Bandit Level 12 → Level 13

## 🎯 Goal

The password for the next level is stored in the file `data.txt`, which is a hex dump of a file that has been repeatedly compressed.

**Connect with:** `ssh bandit12@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How to reverse a hex dump using `xxd -r`
- How to identify file types using the `file` command on binary data
- How to decompress files in multiple formats: `gzip`, `bzip2`, `tar`
- The importance of working in a writable temporary directory
- Iterative problem-solving when layers of compression are stacked

---

## 🛠 Commands Used

```bash
# Create a working directory in /tmp
mkdir /tmp/mywork
cp data.txt /tmp/mywork/
cd /tmp/mywork

# Reverse the hex dump to get the binary file
xxd -r data.txt > data.bin

# Identify file type
file data.bin

# Decompress (repeat as needed based on file type)
mv data.bin data.gz && gunzip data.gz          # gzip
mv data data.bz2 && bunzip2 data.bz2           # bzip2
mv data data.tar && tar xf data.tar            # tar archive
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Set up a working directory**

The home directory of bandit12 is not writable. Create a temp workspace:

```bash
mkdir /tmp/mywork12
cp ~/data.txt /tmp/mywork12/
cd /tmp/mywork12
```

**Step 2 — Reverse the hex dump**

`data.txt` is a hex dump (human-readable hex representation of binary data). Convert it back to binary:

```bash
xxd -r data.txt > data.bin
```

`xxd -r` reverses a hex dump into binary.

**Step 3 — Peel the compression layers**

This level has multiple nested compression layers. Use `file` at each step to identify the format, then decompress accordingly:

```bash
file data.bin
# → data.bin: gzip compressed data
mv data.bin data.gz
gunzip data.gz
file data
# → data: bzip2 compressed data
mv data data.bz2
bunzip2 data.bz2
file data
# → data: gzip compressed data ...
```

Continue this process. You will encounter gzip, bzip2, and tar formats in various orders. Keep using `file` to identify each layer and use the appropriate tool:

| Format | Extension | Command |
|--------|-----------|---------|
| gzip | `.gz` | `gunzip file.gz` or `gzip -d file.gz` |
| bzip2 | `.bz2` | `bunzip2 file.bz2` |
| tar | `.tar` | `tar xf file.tar` |

**Step 4 — Final result**

Eventually `file` will report: `ASCII text`. Use `cat` to read the password.

---

## 🔐 Cybersecurity / SOC Relevance

Understanding compression and encoding layers is essential in malware analysis and forensics. Attackers use multiple compression layers to:
- **Evade antivirus** — signature-based AVs may not scan inside nested archives
- **Obfuscate payloads** — each layer adds analysis friction for defenders
- **Reduce file size** — for rapid deployment or exfiltration

In incident response, you may find suspicious archives on compromised systems. The workflow of iteratively running `file` and decompressing mirrors real forensic analysis. Malware packers like UPX, MPRESS, or custom packers create a similar "peeling" experience.

Hex dumps (`xxd`) are also used in log analysis and memory forensics when raw binary data needs to be reviewed or shared as text.

---

## 💡 Key Takeaways

- `xxd -r` converts a hex dump back to binary; `xxd` (without `-r`) creates a hex dump
- Always run `file` before trying to decompress — it tells you exactly what tool to use
- Multiple compression layers are common in malware; patience and methodology matter
- Work in `/tmp` when your home directory isn't writable

---

## ❌ Common Mistakes

- **Trying to decompress without running `file` first** — using the wrong tool causes errors
- **Forgetting to rename files with the correct extension** — `gunzip` requires a `.gz` extension; `tar` is more flexible with `tar xf`
- **Working in the home directory** — it's not writable; always move to `/tmp`
- **Giving up early** — there are many layers; keep checking with `file` until you reach ASCII text
