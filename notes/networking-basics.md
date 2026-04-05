# 🌐 Networking Basics — Reference Notes

> Core networking concepts every SOC Analyst and cybersecurity learner should understand.

---

## 🔌 IP Addresses

An **IP address** uniquely identifies a device on a network.

| Type | Range | Use |
|---|---|---|
| **Private IPv4** | `10.x.x.x`, `172.16-31.x.x`, `192.168.x.x` | Internal networks |
| **Public IPv4** | Everything else | Internet-facing |
| **Loopback** | `127.0.0.1` | Refers to the local machine itself |
| **IPv6** | `::1` (loopback), etc. | Modern internet addressing |

---

## 🚪 Ports

Ports are like "doors" on a host — each service listens on a specific port number.

| Port | Protocol | Service |
|---|---|---|
| 22 | TCP | SSH |
| 23 | TCP | Telnet (insecure) |
| 25 | TCP | SMTP (email) |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP (Windows Remote Desktop) |
| 8080 | TCP | HTTP alternative / proxy |

**Port ranges:**
- `0–1023` — Well-known ports (system/privileged)
- `1024–49151` — Registered ports
- `49152–65535` — Dynamic/ephemeral ports

---

## 📡 Common Protocols

| Protocol | Layer | Purpose |
|---|---|---|
| **TCP** | Transport | Reliable, connection-oriented (handshake) |
| **UDP** | Transport | Fast, connectionless (no guarantee of delivery) |
| **HTTP/HTTPS** | Application | Web traffic |
| **SSH** | Application | Encrypted remote access |
| **DNS** | Application | Resolves domain names to IPs |
| **ICMP** | Network | Ping, traceroute |

---

## 🔐 SSH — Secure Shell

SSH provides encrypted remote access to a server.

```bash
# Basic connection
ssh username@host

# Specify a port
ssh username@host -p 2220

# Connect using a key file
ssh -i ~/.ssh/id_rsa username@host

# Port forwarding (tunnel local port to remote)
ssh -L 8080:localhost:80 user@server

# Generate SSH key pair
ssh-keygen -t ed25519 -C "your_email@example.com"
```

**Key files:**
- `~/.ssh/id_rsa` or `~/.ssh/id_ed25519` — Private key (keep secret, `600` permissions)
- `~/.ssh/id_rsa.pub` — Public key (shared with server)
- `~/.ssh/authorized_keys` — Server stores public keys of allowed clients
- `~/.ssh/known_hosts` — Client stores server fingerprints

---

## 🔧 Network Diagnostic Commands

```bash
# Check connectivity
ping google.com
ping -c 4 192.168.1.1       # send exactly 4 packets

# Trace the route packets take
traceroute google.com
tracepath google.com         # alternative

# DNS lookup
nslookup google.com
dig google.com
host google.com

# Show network interfaces and IPs
ip addr show
ifconfig                     # older alternative

# Show routing table
ip route show
netstat -rn

# Show active connections and listening ports
ss -tulnp                    # modern replacement for netstat
netstat -tulnp               # older alternative

# Scan open ports on a host (with nmap)
nmap -sV 192.168.1.1         # version detection
nmap -p 22,80,443 host.com   # specific ports
```

---

## 🐈 Netcat (`nc`) — The Swiss Army Knife

Netcat opens raw TCP/UDP connections — used for debugging, banner grabbing, and more.

```bash
# Connect to a server (like a basic telnet)
nc bandit.labs.overthewire.org 30000

# Listen for incoming connections
nc -lvnp 4444

# Transfer a file
nc -lvnp 4444 > received_file.txt           # receiver
nc 192.168.1.100 4444 < file_to_send.txt    # sender

# Banner grabbing (identify service version)
nc -nv 192.168.1.1 22
```

---

## 🔒 OpenSSL

OpenSSL is used for encryption, certificates, and connecting to TLS-protected services.

```bash
# Connect to a TLS/SSL service
openssl s_client -connect host:port

# Encode/decode Base64
echo "hello" | openssl base64
echo "aGVsbG8K" | openssl base64 -d

# Generate a random password
openssl rand -base64 32
```

---

## 🧠 SOC Analyst Relevance

| Concept | Real-World Application |
|---|---|
| **Port scanning** | Detecting unauthorized services (e.g., unexpected port 4444 = possible backdoor) |
| **SSH keys** | Investigating unauthorized `authorized_keys` additions (persistence mechanism) |
| **Unusual ports** | Malware often communicates on non-standard ports to evade detection |
| **DNS** | DNS tunneling is a common covert channel for data exfiltration |
| **Netcat** | Attackers use `nc` to create reverse shells and transfer files |
| **HTTPS inspection** | SOCs may inspect TLS traffic for malware C2 communication |
