# Bandit Level 7 → Level 8

## 🎯 Goal

The password for the next level is stored in the file `data.txt` next to the word **millionth**.

**Connect with:** `ssh bandit7@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How to search for a specific string within a file using `grep`
- The concept of pattern matching in text files
- How `grep` outputs the entire line containing a match
- Piping output from one command into another

---

## 🛠 Commands Used

```bash
# Find the line containing "millionth"
grep "millionth" data.txt

# Alternative: use pipes
cat data.txt | grep "millionth"
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Assess the file**

```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
ls
wc -l data.txt
```

The file `data.txt` contains hundreds of thousands of lines — far too many to read manually.

**Step 2 — Use `grep` to search**

`grep` (Global Regular Expression Print) searches for lines matching a pattern:

```bash
grep "millionth" data.txt
```

`grep` reads through all lines in `data.txt` and prints only the lines that contain the word "millionth". 

Output:
```
millionth	<the_password_here>
```

The password is right there, on the same line as "millionth".

**Step 3 — Using pipes (alternative)**

Instead of passing the filename directly to grep, you can pipe the output of `cat`:

```bash
cat data.txt | grep "millionth"
```

The `|` (pipe) sends the stdout of `cat` as stdin to `grep`. Both approaches give the same result, but the direct filename method is more efficient.

---

## 🔐 Cybersecurity / SOC Relevance

`grep` is arguably the most-used tool by SOC Analysts. Every day, analysts grep through:
- System logs to find events matching an IP address: `grep "192.168.1.5" /var/log/auth.log`
- Access logs to find a specific user's requests: `grep "admin" /var/log/apache2/access.log`
- Firewall logs to find blocked connections to suspicious domains
- Configuration files to find hardcoded credentials: `grep -r "password" /etc/`

The ability to quickly extract relevant information from massive log files using `grep` is a fundamental SOC skill that directly impacts how fast you can respond to an incident.

---

## 💡 Key Takeaways

- `grep "pattern" file` prints all lines in `file` that contain "pattern"
- `grep` is case-sensitive by default; use `-i` for case-insensitive matching
- `grep -r "pattern" /directory/` searches recursively through directories
- The pipe `|` connects commands: stdout of the left command → stdin of the right command

---

## ❌ Common Mistakes

- **Using `cat` and reading manually** — the file has too many lines; grep is the right tool
- **Forgetting quotes around the search term** — for single words it works without quotes, but always quote multi-word patterns
- **Case sensitivity** — if unsure of capitalization, add the `-i` flag: `grep -i "millionth" data.txt`
