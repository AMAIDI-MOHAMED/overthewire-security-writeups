# Bandit Level 2 → Level 3

## 🎯 Goal

The password for the next level is stored in a file called `spaces in this filename` located in the home directory.

**Connect with:** `ssh bandit2@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How to handle filenames that contain spaces in the Linux shell
- Two methods: quoting and backslash escaping
- How shell tokenization works and why spaces are special characters

---

## 🛠 Commands Used

```bash
# List files
ls

# Method 1: Wrap the filename in quotes
cat "spaces in this filename"

# Method 2: Escape each space with a backslash
cat spaces\ in\ this\ filename

# Method 3: Use Tab autocompletion (most practical)
cat spaces<Tab>
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Connect and list files**

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
ls
```

Output:
```
spaces in this filename
```

**Step 2 — Understand the problem**

The shell uses spaces to separate arguments. If you type:
```bash
cat spaces in this filename
```
The shell sees **four separate arguments** (`spaces`, `in`, `this`, `filename`) and tries to open four different files — none of which exist. You'll see: `No such file or directory`.

**Step 3 — Quote the filename**

Wrapping the entire name in quotes tells the shell to treat it as a single argument:
```bash
cat "spaces in this filename"
```

Single quotes also work:
```bash
cat 'spaces in this filename'
```

**Step 4 — Alternatively, escape the spaces**

A backslash (`\`) before a space tells the shell "this space is part of the argument, not a separator":
```bash
cat spaces\ in\ this\ filename
```

**Pro tip:** Type `cat sp` then press **Tab** — the shell will autocomplete the filename and automatically escape the spaces for you.

---

## 🔐 Cybersecurity / SOC Relevance

Malicious files discovered during incident response sometimes use spaces in their names to trick analysts or automated tools. For example, a malware file named `system update.exe` might look like two words but is actually a single filename. Scripts and security tools that don't properly quote filenames can break or behave unexpectedly.

In log analysis and SIEM rules, improper handling of spaces in field values is a source of parsing errors. Understanding shell quoting protects you from these pitfalls.

---

## 💡 Key Takeaways

- The Linux shell splits arguments on spaces by default — quotes override this
- Both single quotes (`'`) and double quotes (`"`) work for filenames with spaces
- Backslash escaping (`\ `) is an alternative to quoting
- Tab autocomplete is the fastest and most reliable approach in practice

---

## ❌ Common Mistakes

- **Forgetting to quote** — results in "No such file or directory" errors for each word
- **Not using Tab completion** — it would have saved you the trouble entirely
- **Mixing up single and double quotes** — both work here, but double quotes expand variables while single quotes do not (important to know for scripting)
