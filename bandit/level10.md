# 🔐 Bandit Level 10

## 🎯 Goal

The password is stored in `data.txt`, which contains **Base64-encoded** data.

- **Login:** `ssh bandit10@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 9)*

---

## 🧠 What I Learned

- What Base64 encoding is and why it exists
- How to decode Base64 data on the command line
- The difference between encoding and encryption

---

## 🛠 Commands Used

```bash
cat data.txt
base64 -d data.txt
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Read the file**

```bash
cat data.txt
```

Output:
```
VGhlIHBhc3N3b3JkIGlzIDZ6UGV6aUxkUjhSM1hXa3UxaVRBek1BU21YcVpFZUhG
```

This is a Base64-encoded string — readable characters but not plain text.

**Step 2 — Decode the Base64 data**

```bash
base64 -d data.txt
```

- `base64` — the Base64 utility
- `-d` — decode mode (default is encode)

Output:
```
The password is <password>
```

---

## 🔐 Cybersecurity / SOC Relevance

Base64 is extremely common in security contexts — and very commonly abused:

- **Malware obfuscation**: Attackers encode malicious payloads, shellcode, or commands in Base64 to evade signature-based detection
  ```
  powershell -EncodedCommand <base64_string>
  ```
- **Phishing emails**: Malicious attachments or links are often Base64-encoded in email headers (MIME format)
- **Web attacks**: Base64-encoded payloads appear in XSS, SQL injection, and command injection attempts in web logs
- **Data exfiltration**: Stolen data is sometimes Base64-encoded before being sent over HTTP to blend in with normal traffic

**⚠️ Important distinction**: Base64 is **encoding**, NOT **encryption**. It is completely reversible with no key. It provides obfuscation, not confidentiality.

As a SOC Analyst, recognizing Base64 strings and decoding them is a daily skill — especially when investigating PowerShell commands in Windows event logs or suspicious HTTP requests.

---

## 💡 Key Takeaways

- Base64 uses only characters: `A-Z`, `a-z`, `0-9`, `+`, `/`, and `=` (padding)
- `base64 -d` decodes; `base64` (no flag) encodes
- Base64 ≠ encryption — it offers zero security
- Long Base64 strings in logs, emails, or scripts are always worth investigating

---

## ❌ Common Mistakes

- Thinking Base64 is a form of encryption — it is not
- Forgetting the `-d` flag (without it, you re-encode the already-encoded data)
- Confusing Base64 with hex encoding (hex uses only `0-9` and `a-f`)
