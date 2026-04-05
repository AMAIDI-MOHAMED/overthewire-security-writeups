# 🔐 Bandit Level 7

## 🎯 Goal

The password is stored in the file `data.txt` next to the word `millionth`.

- **Login:** `ssh bandit7@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 6)*

---

## 🧠 What I Learned

- How to search inside file content with `grep`
- How `grep` returns the entire line containing a match
- Efficient text searching vs. reading files manually

---

## 🛠 Commands Used

```bash
grep "millionth" data.txt
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Inspect the file**

```bash
wc -l data.txt
```

Output:
```
98567 data.txt
```

The file has nearly 100,000 lines — far too many to read manually.

**Step 2 — Search for the keyword**

```bash
grep "millionth" data.txt
```

Output:
```
millionth       <password>
```

`grep` scans every line and prints only those containing the search term. The password appears right next to the word `millionth`.

---

## 🔐 Cybersecurity / SOC Relevance

`grep` is one of the most-used tools in a SOC Analyst's daily work:

- **Log analysis**: Quickly find error messages, authentication failures, or specific IPs in large log files
  ```bash
  grep "Failed password" /var/log/auth.log
  grep "192.168.1.100" access.log
  ```
- **IOC hunting**: Search for Indicators of Compromise (suspicious IPs, domains, hashes) across multiple files
- **SIEM alternative**: For quick ad-hoc searches when a full SIEM query is overkill

Mastering `grep` (including flags like `-i`, `-r`, `-n`, `-v`) is essential for fast, effective incident response.

---

## 💡 Key Takeaways

- `grep "pattern" file` — searches for a pattern in a file and prints matching lines
- `-i` makes the search case-insensitive
- `-n` shows line numbers for each match
- `-r` searches recursively through directories
- `-v` inverts the match (show lines that do NOT contain the pattern)

---

## ❌ Common Mistakes

- Using `cat data.txt | grep "millionth"` — works, but `grep "millionth" data.txt` is more efficient (no unnecessary pipe)
- Forgetting to quote patterns with spaces or special characters
