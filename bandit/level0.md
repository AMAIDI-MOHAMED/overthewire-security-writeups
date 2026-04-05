# 🔐 Bandit Level 0

## 🎯 Goal

Connect to the Bandit game server using SSH to access the first challenge.

- **Host:** `bandit.labs.overthewire.org`
- **Port:** `2220`
- **Username:** `bandit0`
- **Password:** `bandit0`

---

## 🧠 What I Learned

- How SSH works at a basic level
- The meaning of SSH connection parameters (`-p`, host, user)
- What a "wargame" environment is and how to interact with a remote Linux shell

---

## 🛠 Commands Used

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Once connected, read the password for the next level:

```bash
cat readme
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Connect via SSH**

SSH (Secure Shell) allows you to remotely control another machine over an encrypted connection. The basic syntax is:

```
ssh <username>@<host> -p <port>
```

- `bandit0` is both the username and password for this entry level
- Port `2220` is used instead of the default SSH port `22`

**Step 2 — Accept the host fingerprint**

On first connection, SSH asks you to confirm the server's fingerprint. Type `yes` to proceed. This is a trust-on-first-use (TOFU) mechanism.

**Step 3 — Read the password file**

Once logged in, list the files:

```bash
ls
```

Output:
```
readme
```

Read its content:

```bash
cat readme
```

This reveals the password for **Level 1**.

---

## 🔐 Cybersecurity / SOC Relevance

SSH is one of the most common protocols used in both legitimate administration and malicious intrusions. As a SOC Analyst, you will:

- Monitor SSH login attempts (especially failed ones) in SIEM dashboards
- Identify brute-force attacks against SSH ports in firewall logs
- Investigate unusual SSH activity from unknown IPs or at odd hours

Understanding how SSH works from the user's perspective makes it easier to recognize malicious patterns in logs.

---

## 💡 Key Takeaways

- SSH uses port `22` by default; non-standard ports (like `2220`) are used to reduce automated scanning noise
- Always verify the host fingerprint when connecting to a new server
- `cat` is the simplest way to read the content of a file

---

## ❌ Common Mistakes

- Forgetting the `-p 2220` flag → connection refused
- Typing `SSH` with capital letters (the command is lowercase `ssh`)
- Not accepting the fingerprint prompt (typing `no` disconnects you)
