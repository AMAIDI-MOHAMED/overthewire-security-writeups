# Bandit Level 0 → Level 1

## 🎯 Goal

Connect to the Bandit game server via SSH using the provided credentials and read the password for the next level stored in a file called `readme` in the home directory.

**Host:** `bandit.labs.overthewire.org`
**Port:** `2220`
**Username:** `bandit0`
**Password:** `bandit0`

---

## 🧠 What I Learned

- How SSH (Secure Shell) works and why it is the standard for remote system access
- The meaning of SSH flags like `-p` for specifying a non-default port
- How to read a file using `cat`
- The concept of a home directory and how to navigate to it

---

## 🛠 Commands Used

```bash
# Connect to the remote server via SSH on a non-standard port
ssh bandit0@bandit.labs.overthewire.org -p 2220

# Read the contents of the readme file
cat readme
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Connect via SSH**

SSH (Secure Shell) is a protocol that lets you securely log into a remote computer over a network. The standard SSH port is 22, but Bandit uses port 2220 to avoid conflicts. We specify this with the `-p` flag.

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

- `bandit0` — the username we're logging in as
- `@bandit.labs.overthewire.org` — the remote host address
- `-p 2220` — connect on port 2220 instead of the default port 22

When prompted for a password, type `bandit0` and press Enter.

**Step 2 — Look around**

Once connected, you land in the home directory of `bandit0`. List the files to see what's there:

```bash
ls
```

Output:
```
readme
```

**Step 3 — Read the file**

```bash
cat readme
```

The password for `bandit1` is displayed. Copy it — you'll need it to connect to the next level.

---

## 🔐 Cybersecurity / SOC Relevance

SSH is the backbone of remote system administration in most enterprise and cloud environments. As a SOC Analyst, you will regularly:

- SSH into compromised systems to investigate incidents
- Review SSH logs (`/var/log/auth.log`) for brute-force attempts or unauthorized logins
- Identify unusual SSH connections in SIEM alerts (e.g., logins from foreign IPs, off-hours access)

Understanding how SSH works — authentication, key exchange, port usage — is essential for detecting and responding to SSH-based attacks.

---

## 💡 Key Takeaways

- SSH is used to securely connect to remote Linux systems
- Use `-p` to specify a custom port
- The `cat` command outputs the full contents of a text file to the terminal
- Your home directory is where you start when you log in; use `ls` to see what's there

---

## ❌ Common Mistakes

- **Forgetting the `-p 2220` flag** — SSH defaults to port 22; connecting without `-p` will fail
- **Typing the password incorrectly** — SSH won't show characters as you type; type carefully
- **Closing the terminal without saving the password** — Always note down the found password before disconnecting
