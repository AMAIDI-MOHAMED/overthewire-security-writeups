# Bandit Level 13 → Level 14

## 🎯 Goal

The password for the next level is stored in `/etc/bandit_pass/bandit14` and can only be read by user `bandit14`. Instead of a password, this level provides a **private SSH key** to log in as `bandit14`.

**Connect with:** `ssh bandit13@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How SSH key-based authentication works
- The difference between password authentication and key-based authentication
- How to use a private key file with the `-i` flag in SSH
- The concept of public/private key pairs and why they're more secure than passwords

---

## 🛠 Commands Used

```bash
# List the available file
ls

# Use the private key to SSH into bandit14 (from within the bandit13 session)
ssh -i sshkey.private bandit14@localhost -p 2220

# Once logged in as bandit14, read the password
cat /etc/bandit_pass/bandit14
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Discover the key**

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
ls
```

Output:
```
sshkey.private
```

A private SSH key file is present. We can use it to authenticate as `bandit14` without needing a password.

**Step 2 — Understand SSH key authentication**

SSH supports two authentication methods:
1. **Password authentication** — you type a password (can be brute-forced)
2. **Key-based authentication** — you have a private key file; the server holds the matching public key

The server's `authorized_keys` file for `bandit14` contains the public key that matches `sshkey.private`. When we present the private key, SSH proves we own the corresponding key pair via a cryptographic challenge — no password needed.

**Step 3 — SSH using the key**

From within the bandit13 session, connect to `bandit14` on the same server (`localhost`):

```bash
ssh -i sshkey.private bandit14@localhost -p 2220
```

- `-i sshkey.private` — specifies the identity file (private key)
- `bandit14@localhost` — connect to localhost as user bandit14
- `-p 2220` — use port 2220

**Step 4 — Read the password**

Now logged in as bandit14:
```bash
cat /etc/bandit_pass/bandit14
```

The password is displayed.

---

## 🔐 Cybersecurity / SOC Relevance

SSH key-based authentication is the **recommended standard** for server access in production environments — password authentication is often disabled entirely. As a SOC Analyst, understanding SSH keys is critical for:

- **Investigating unauthorized access** — attackers who compromise a server often add their public key to `~/.ssh/authorized_keys` to maintain persistent access even if passwords are changed
- **Detecting lateral movement** — an attacker may steal a private key from one host and use it to pivot to other servers
- **Key management audits** — reviewing `authorized_keys` files across systems to identify unauthorized or orphaned keys
- **Incident response** — revoking compromised keys as part of containment

SSH private keys found on a compromised system (especially without a passphrase) are high-severity findings that should be immediately investigated.

---

## 💡 Key Takeaways

- `-i keyfile` tells SSH which private key to use for authentication
- Key-based auth is more secure than passwords: no brute-force risk, no password to leak
- The private key must have permissions `600` or `400` — SSH refuses to use world-readable keys
- `localhost` means "this machine" — used to SSH into the same host under a different user
- Attackers frequently add unauthorized keys to `authorized_keys` for persistence

---

## ❌ Common Mistakes

- **Permissions too open** — if your key file is world-readable (`chmod 644`), SSH will refuse it with "Permissions are too open." Fix with `chmod 400 sshkey.private`
- **Forgetting `-p 2220`** — the custom port is still needed even when connecting to localhost
- **Confusing public and private keys** — you authenticate with the **private** key; the **public** key lives on the server
