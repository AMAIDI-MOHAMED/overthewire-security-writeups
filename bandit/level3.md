# Bandit Level 3 → Level 4

## 🎯 Goal

The password for the next level is stored in a hidden file in the `inhere` directory.

**Connect with:** `ssh bandit3@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How hidden files work in Linux (the dot prefix convention)
- How to reveal hidden files with `ls -a` or `ls -la`
- The difference between `ls` and `ls -a`
- Navigating into subdirectories

---

## 🛠 Commands Used

```bash
# List visible files
ls

# Navigate into the inhere directory
cd inhere

# List ALL files including hidden ones
ls -la

# Read the hidden file
cat .hidden
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Connect and explore**

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
ls
```

Output:
```
inhere
```

There's a directory called `inhere`. Navigate into it:

```bash
cd inhere
ls
```

Output: nothing. The directory appears empty!

**Step 2 — Show hidden files**

In Linux, any file or directory whose name starts with a `.` (dot) is hidden from the default `ls` output. To reveal hidden files, use the `-a` flag (all):

```bash
ls -a
```

Output:
```
.  ..  .hidden
```

- `.` — the current directory
- `..` — the parent directory
- `.hidden` — our target file!

**Step 3 — Read the hidden file**

```bash
cat .hidden
```

The password for `bandit4` is displayed.

**Tip:** `ls -la` combines `-l` (long format, shows permissions/size/date) and `-a` (show hidden). It's the most informative way to list directory contents:

```bash
ls -la
```

---

## 🔐 Cybersecurity / SOC Relevance

Attackers frequently hide malicious files using the dot-prefix convention on Linux systems. During forensic investigations and incident response, you must **always check for hidden files** — never trust a plain `ls` output. Common hiding spots include:

- `.bash_history`, `.bash_profile` — sometimes tampered with to cover tracks
- Hidden directories in `/tmp` or `/home/user/` — used to store tools or exfiltrated data
- Dotfiles in web server directories — hidden config or backdoor scripts

Running `ls -la` on suspicious directories is a basic but essential step in any Linux investigation.

---

## 💡 Key Takeaways

- Files starting with `.` are hidden from default `ls` — use `ls -a` to reveal them
- `ls -la` is the gold standard: shows all files with full details
- `cd` navigates into directories; `pwd` shows your current location
- Hidden does NOT mean secure — it is purely a display convention

---

## ❌ Common Mistakes

- **Stopping at `ls` when the directory looks empty** — always follow up with `ls -a`
- **Confusing hidden with encrypted** — a `.hidden` file is just hidden from view, not protected
- **Forgetting the dot when reading the file** — `cat hidden` won't work; it must be `cat .hidden`
