# 🔐 Bandit Level 9

## 🎯 Goal

The password is stored in `data.txt` among binary data, in one of the few human-readable strings, preceded by several `=` characters.

- **Login:** `ssh bandit9@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 8)*

---

## 🧠 What I Learned

- How to extract human-readable strings from binary files using `strings`
- How to combine `strings` with `grep` to target specific patterns
- Why binary files can't simply be `cat`-ed

---

## 🛠 Commands Used

```bash
strings data.txt | grep "==="
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Try to read the file directly**

```bash
cat data.txt
```

The output is mostly garbage — binary data mixed with a few readable fragments. Scrolling through it manually is not practical.

**Step 2 — Extract human-readable strings**

The `strings` command scans a binary file and outputs sequences of printable characters (by default, 4+ characters long):

```bash
strings data.txt
```

This produces a more manageable list of readable fragments.

**Step 3 — Filter for the password hint**

The problem states the password is preceded by `=` characters. Filter for lines containing `===`:

```bash
strings data.txt | grep "==="
```

Output:
```
========== the
========== password
========== is
========== <password>
```

The last match is the password for Level 10.

---

## 🔐 Cybersecurity / SOC Relevance

`strings` is a fundamental tool in **malware analysis** and **forensics**:

- **Static malware analysis**: Analysts run `strings` on suspicious binaries to extract URLs, IP addresses, registry keys, error messages, and C2 domain names — all without executing the malware
- **Firmware analysis**: Extract readable content from embedded firmware images
- **Packed/obfuscated executables**: Even obfuscated malware often leaves readable strings in memory sections

Example workflow in a SOC:
```bash
strings suspicious_binary.exe | grep -E "(http|ftp|\.com|\.ru)"
strings suspicious_binary.exe | grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}"
```

---

## 💡 Key Takeaways

- `strings` extracts printable character sequences from any file — especially useful for binaries
- Combine with `grep` to narrow results to relevant patterns
- `-n <length>` sets the minimum string length (default is 4)
- This is a basic static analysis step performed on every suspicious file

---

## ❌ Common Mistakes

- Running `cat` on a binary file — garbles the terminal (use `reset` to recover)
- Not piping `strings` output to `grep` — too many lines to review manually
