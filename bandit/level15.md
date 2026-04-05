# Bandit Level 15 → Level 16

## 🎯 Goal

The password for the next level can be retrieved by submitting the current level's password to **port 30001** on `localhost` using **SSL/TLS encryption**.

**Connect with:** `ssh bandit15@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- What SSL/TLS is and why it's used to secure network connections
- How to connect to an SSL/TLS-encrypted service using `openssl s_client`
- The difference between a plain TCP connection (netcat) and an encrypted TLS connection
- Why plain-text password transmission is dangerous

---

## 🛠 Commands Used

```bash
# Connect to port 30001 using SSL/TLS
openssl s_client -connect localhost:30001

# Then type/paste the password and press Enter

# Non-interactive version
echo "$(cat /etc/bandit_pass/bandit15)" | openssl s_client -connect localhost:30001 -quiet
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Why netcat won't work here**

If you try `nc localhost 30001` and send the password, the connection will fail or produce garbled output. Port 30001 expects an SSL/TLS handshake before any data exchange — plain `nc` doesn't speak SSL.

**Step 2 — Understand SSL/TLS**

**SSL** (Secure Sockets Layer) and its successor **TLS** (Transport Layer Security) are cryptographic protocols that add encryption, integrity, and authentication to network connections.

Without TLS, data sent over the network is plaintext — anyone on the same network can read it (a "man-in-the-middle" attack). With TLS:
- Data is encrypted in transit
- The server's identity is verified via a certificate
- Data cannot be tampered with undetected

HTTPS = HTTP over TLS. SSH uses its own encryption (not TLS). Port 30001 uses a custom TLS-wrapped service.

**Step 3 — Connect with `openssl s_client`**

`openssl s_client` is a diagnostic tool for connecting to TLS services. It performs the TLS handshake and then allows interactive communication:

```bash
openssl s_client -connect localhost:30001
```

You'll see extensive TLS handshake output (certificate chain, cipher suite, etc.), followed by `---` indicating the connection is ready for data.

Type or paste the bandit15 password and press Enter.

The server responds:
```
Correct!
The password for bandit16 is: <the_next_password>
```

**Using `-quiet` for cleaner output:**
```bash
echo "$(cat /etc/bandit_pass/bandit15)" | openssl s_client -connect localhost:30001 -quiet
```

`-quiet` suppresses the TLS handshake information and shows only application data.

---

## 🔐 Cybersecurity / SOC Relevance

TLS is everywhere in modern security:

- **HTTPS traffic inspection** — SOC Analysts work with TLS-encrypted web traffic. Understanding TLS is essential for interpreting SSL inspection logs in proxy solutions
- **Certificate analysis** — expired, self-signed, or suspicious certificates are indicators of attack; `openssl s_client` is used to inspect certificates manually
- **Malware C2 over HTTPS** — modern malware uses TLS to encrypt its command-and-control traffic, making it blend in with normal web traffic
- **TLS fingerprinting (JA3)** — SOC teams use TLS client fingerprints to identify malicious software even when content is encrypted

Running `openssl s_client -connect hostname:443` is a quick way to inspect a server's certificate, check for weak cipher suites, or debug TLS connection issues.

---

## 💡 Key Takeaways

- `openssl s_client -connect host:port` opens a TLS-encrypted connection
- TLS provides encryption, integrity, and server authentication
- Plain `nc` cannot speak TLS — use `openssl s_client` or `ncat --ssl` for encrypted services
- `-quiet` flag in `openssl s_client` suppresses handshake output for cleaner piping

---

## ❌ Common Mistakes

- **Using `nc` for an SSL service** — it will connect but SSL handshake won't happen; the service won't respond meaningfully
- **Sending the wrong level's password** — it must be bandit15's password, not bandit14's
- **Panicking at the verbose TLS output** — the handshake output is just informational; scroll past it or use `-quiet`
- **Not pressing Enter after the password** — the service waits for a newline to process the input
