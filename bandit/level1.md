# 🔐 Bandit Level 1

## 🎯 Goal

Read the file named `-` (a single dash) to retrieve the password for Level 2.

- **Login:** `ssh bandit1@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 0)*

---

## 🧠 What I Learned

- How the shell interprets arguments that begin with `-` as flags/options
- How to reference files with special names using path prefixes
- The difference between a filename and a command-line option

---

## 🛠 Commands Used

```bash
ls
cat ./-
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — List files**

```bash
ls
```

Output:
```
-
```

There is a single file named `-`.

**Step 2 — Understand the problem**

If you try `cat -`, the shell interprets `-` as a special argument meaning "read from standard input" rather than as a filename. The command will hang waiting for keyboard input.

**Step 3 — Use a path prefix to force filename interpretation**

By prepending `./` (current directory), we tell the shell this is a path, not a flag:

```bash
cat ./-
```

This outputs the password for Level 2.

---

## 🔐 Cybersecurity / SOC Relevance

Attackers sometimes create files or directories with misleading names (e.g., starting with `-`, containing spaces, or resembling system paths) to:

- Trick administrators into accidentally executing them
- Evade naive log parsers or scripts that split on spaces or dashes

Knowing how the shell parses arguments helps a SOC Analyst build more robust scripts for log analysis and automated response — and understand attacker obfuscation techniques.

---

## 💡 Key Takeaways

- Files starting with `-` are valid on Linux, but require special handling
- Always use `./filename` when dealing with files whose names could be misinterpreted as options
- Alternatively, use `cat -- -` (the `--` signals end of options in many commands)

---

## ❌ Common Mistakes

- Running `cat -` and waiting forever (the shell is reading stdin, not the file)
- Trying to delete such a file with `rm -` without the `./` prefix
