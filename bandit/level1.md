# Bandit Level 1 → Level 2

## 🎯 Goal

The password for the next level is stored in a file called `-` (a single dash) located in the home directory.

**Connect with:** `ssh bandit1@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- Why certain filenames are problematic for standard shell commands
- How to reference files whose names begin with special characters
- How the shell interprets `-` as a flag/option prefix vs. an actual filename
- Alternative ways to refer to the current directory

---

## 🛠 Commands Used

```bash
# List files in the home directory
ls

# Read a file named "-" using a path prefix
cat ./-

# Alternative: use input redirection
cat < -
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Connect and explore**

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
ls
```

Output:
```
-
```

There's a file literally named `-`. Seems straightforward… until you try:

```bash
cat -
```

This causes the terminal to hang because `cat` interprets `-` as "read from standard input" (keyboard), not as a filename. This is a convention in Unix tools — `-` means stdin/stdout.

**Step 2 — Use a path to be explicit**

To tell the shell "this is a file in the current directory, not a flag," prefix the filename with `./`:

```bash
cat ./-
```

The `./` means "current directory," so `./-` unambiguously refers to the file named `-` in the current working directory. The password is displayed.

---

## 🔐 Cybersecurity / SOC Relevance

Attackers sometimes use unusual or misleading filenames to hide malicious files — names starting with `-`, spaces, or invisible Unicode characters. During incident response, a SOC Analyst examining a filesystem may encounter such files and need to know how to safely read or delete them without the shell misinterpreting them.

This also relates to **argument injection vulnerabilities**: if a web application passes user input directly to a shell command, a user could name a file `--flag` and manipulate command behavior — a classic injection scenario.

---

## 💡 Key Takeaways

- In Unix, `-` has a special meaning for many commands (stdin/stdout); it is NOT treated as a filename
- Use `./` to explicitly reference files in the current directory, especially when names start with `-`
- The `<` redirection operator (`cat < -`) is another valid approach
- Always be careful about how shells interpret special characters in filenames

---

## ❌ Common Mistakes

- **Running `cat -` directly** — the terminal will hang waiting for keyboard input
- **Trying `cat "-"`** — quoting the filename alone does not help here; `-` is still interpreted as stdin by `cat`
- **Not understanding why `./` works** — it forces the argument to be parsed as a path, not an option
