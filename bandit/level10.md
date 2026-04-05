# Bandit Level 10 → Level 11

## 🎯 Goal

The password for the next level is stored in the file `data.txt`, which contains base64-encoded data.

**Connect with:** `ssh bandit10@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- What Base64 encoding is and why it is used
- How to decode Base64 data using the `base64` command
- The critical distinction between **encoding** and **encryption**
- Why encoded data is NOT secure

---

## 🛠 Commands Used

```bash
# View the encoded content
cat data.txt

# Decode the Base64 content
base64 -d data.txt

# Alternative using a pipe
cat data.txt | base64 --decode
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — View the file**

```bash
cat data.txt
```

Output (example):
```
VGhlIHBhc3N3b3JkIGlzIElIZjhqWnp...
```

This looks like random letters and numbers — but it ends with `=` or `==`, which is a telltale sign of Base64 encoding.

**Step 2 — Understand Base64**

Base64 is a way to represent binary data using only 64 printable ASCII characters (`A-Z`, `a-z`, `0-9`, `+`, `/`). It was designed to safely transmit binary data over text-based systems (like email).

Key facts:
- Base64 is **encoding**, not **encryption** — it can be trivially reversed
- The output is approximately 33% larger than the input
- Base64 strings often end in `=` or `==` as padding characters

**Step 3 — Decode the data**

```bash
base64 -d data.txt
```

The `-d` flag tells `base64` to **decode** (default is encode). Output:
```
The password is <the_actual_password>
```

---

## 🔐 Cybersecurity / SOC Relevance

Base64 is heavily used by attackers to obfuscate malicious content:

- **Malware payloads** — PowerShell commands are frequently Base64-encoded to bypass simple string-matching defenses: `powershell -enc <base64string>`
- **Phishing emails** — malicious attachments and HTML content are Base64-encoded in email headers
- **Web shells** — attacker commands embedded in web traffic may be Base64-encoded
- **Log obfuscation** — some attackers encode their commands to avoid triggering signature-based IDS/IPS rules

As a SOC Analyst, recognizing Base64 strings in logs, emails, and traffic is a valuable skill. Decoding them quickly (using `base64 -d` or online tools) reveals the actual payload.

**Red flag indicators:**
- Long strings of seemingly random alphanumeric characters
- Strings ending in `=` or `==`
- PowerShell `-enc` or `-EncodedCommand` flags in process logs

---

## 💡 Key Takeaways

- Base64 is **encoding**, not encryption — it provides zero confidentiality
- `base64 -d` decodes; `base64` (without flags) encodes
- Base64 strings end with `=` padding — a recognizable pattern in logs and traffic
- Encoded ≠ Encrypted: always be skeptical when someone claims encoding is a security measure

---

## ❌ Common Mistakes

- **Confusing encoding with encryption** — Base64 is not a security measure; it is purely a format transformation
- **Forgetting the `-d` flag** — running `base64 data.txt` will encode the already-encoded content again
- **Not recognizing Base64 in the wild** — learn to spot the characteristic character set and `=` padding
