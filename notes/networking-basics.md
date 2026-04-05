# 🌐 Networking Basics — Reference Notes

> Core networking concepts encountered in the OverTheWire Bandit wargame and essential for SOC Analyst work.

---

## 🖥️ What is a Network?

A network is a group of computers connected together so they can communicate. The internet is a global network of networks. In cybersecurity, understanding how data flows between systems — and how that flow can be monitored, blocked, or manipulated — is foundational.

---

## 🔢 IP Addresses

An **IP address** is the unique identifier for a device on a network.

### IPv4

32-bit addresses written as four octets: `192.168.1.100`

- **Private ranges** (not routable on the internet):
  - `10.0.0.0/8`
  - `172.16.0.0/12`
  - `192.168.0.0/16`
- **Loopback**: `127.0.0.1` (also written as `localhost`) — the computer talking to itself
- **Public**: anything outside the private ranges

### IPv6

128-bit addresses: `2001:0db8:85a3::8a2e:0370:7334` — growing in use

---

## 🔌 Ports

A **port** is a number (0–65535) that identifies a specific service running on a computer. Think of it as an apartment number — the IP is the building, the port is the apartment.

### Well-Known Ports (0–1023)

| Port | Protocol | Service |
|------|----------|---------|
| 20, 21 | TCP | FTP (File Transfer) |
| 22 | TCP | SSH (Secure Shell) |
| 23 | TCP | Telnet (insecure!) |
| 25 | TCP | SMTP (Email sending) |
| 53 | UDP/TCP | DNS (Domain Name System) |
| 80 | TCP | HTTP (Web) |
| 110 | TCP | POP3 (Email retrieval) |
| 443 | TCP | HTTPS (Secure Web) |
| 3389 | TCP | RDP (Remote Desktop) |

### Custom/High Ports (1024–65535)

Applications can use any port above 1023. Bandit uses ports like `2220`, `30000`, `30001` for its services.

---

## 🔗 TCP vs UDP

### TCP (Transmission Control Protocol)

- **Connection-oriented**: establishes a connection before data exchange (3-way handshake)
- **Reliable**: guarantees delivery, ordering, error checking
- **Slower** due to overhead
- Used by: SSH, HTTP, HTTPS, FTP, SMTP

**The 3-way handshake:**
```
Client ─── SYN ──────────→ Server
Client ←── SYN-ACK ──────  Server
Client ─── ACK ──────────→ Server
(Connection established)
```

### UDP (User Datagram Protocol)

- **Connectionless**: sends data without establishing a connection first
- **Unreliable**: no delivery guarantee; packets may arrive out of order or not at all
- **Faster** — less overhead
- Used by: DNS, DHCP, video streaming, VoIP

---

## 🔐 SSH (Secure Shell)

SSH is a protocol for securely accessing remote systems over an insecure network. It encrypts all traffic including authentication.

### Basic SSH Usage

```bash
ssh username@hostname              # Connect on default port 22
ssh username@hostname -p 2220      # Connect on a custom port
ssh -i keyfile user@host           # Connect using a private key
ssh -L 8080:localhost:80 user@host # Local port forwarding
```

### SSH Config File (`~/.ssh/config`)

Save connection settings to avoid typing them every time:

```
Host bandit
    HostName bandit.labs.overthewire.org
    User bandit0
    Port 2220
```

Then connect with just: `ssh bandit`

### SSH Key Management

```bash
ssh-keygen -t ed25519            # Generate a new key pair
ssh-copy-id user@host            # Copy your public key to a server
cat ~/.ssh/authorized_keys       # View authorized keys on a server
```

---

## 🛠️ Netcat (`nc`) — The Swiss Army Knife

Netcat can open TCP/UDP connections for testing, file transfer, and exploration.

```bash
nc hostname port               # Connect to a TCP port
nc -l -p 4444                  # Listen on port 4444
nc -u hostname port            # UDP connection
echo "hello" | nc host port    # Send data via pipe
```

**Common security uses:**
```bash
nc -zv hostname 22             # Port scan (check if port is open)
nc -zv hostname 1-1024         # Scan a range of ports
```

---

## 🔒 SSL/TLS

**SSL** (Secure Sockets Layer) and **TLS** (Transport Layer Security) encrypt communications between a client and server. TLS is the modern successor to SSL.

```bash
# Connect to an HTTPS site and inspect the certificate
openssl s_client -connect google.com:443

# Connect to a TLS-wrapped service (quiet mode)
openssl s_client -connect localhost:30001 -quiet

# Check certificate expiry
echo | openssl s_client -connect host:443 2>/dev/null | openssl x509 -noout -dates
```

---

## 🔍 Network Investigation Tools

### `ss` / `netstat` — Show Open Connections

```bash
ss -tulnp            # Show listening TCP/UDP ports
netstat -tulnp       # Older equivalent
ss -tp               # Show established TCP connections
```

### `nmap` — Network Scanner

```bash
nmap hostname                    # Basic port scan
nmap -p 1-65535 hostname         # Scan all ports
nmap -sV hostname                # Detect service versions
nmap -A hostname                 # Aggressive scan (OS, version, scripts)
```

### `curl` / `wget` — HTTP Requests

```bash
curl http://example.com          # GET request
curl -I http://example.com       # Headers only
wget http://example.com/file     # Download a file
```

---

## 🚨 SOC-Relevant Networking Concepts

### Common Attacker Techniques

| Technique | Description |
|-----------|-------------|
| **Port scanning** | Discovering open services on a target |
| **Banner grabbing** | Reading service banners to identify software versions |
| **Reverse shell** | Victim connects back to attacker's listener |
| **Bind shell** | Attacker connects to a shell listener on the victim |
| **Lateral movement** | Moving from one compromised host to another on the same network |
| **Data exfiltration** | Sending stolen data out of the network |

### Important Log Sources for Network Analysis

| Log | Location | What to Look For |
|-----|----------|-----------------|
| SSH auth log | `/var/log/auth.log` | Failed logins, key changes |
| Web access log | `/var/log/apache2/access.log` | Unusual paths, IPs, user-agents |
| Firewall log | `/var/log/ufw.log` or `/var/log/iptables.log` | Blocked/allowed connections |
| DNS log | SIEM or DNS server logs | Unusual domains, high query volume |

---

## 📖 Quick Reference

```bash
ssh user@host -p port          # SSH to remote host
nc host port                   # TCP connection with netcat
openssl s_client -connect h:p  # TLS connection
ss -tulnp                      # Show listening ports
nmap hostname                  # Port scan
curl -I http://example.com     # Check HTTP headers
```

---

> 💡 **SOC Tip:** Always correlate network events with host events. An unusual outbound connection is more suspicious when it coincides with a new process, a modified file, or a failed authentication attempt.
