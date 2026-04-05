# 🔐 OverTheWire Bandit — Security Writeups

> A hands-on cybersecurity learning journal documenting my journey through OverTheWire Bandit, built as part of my portfolio for a **SOC Analyst** career path.

---

## 👤 About Me

I'm a cybersecurity learner actively working toward a career as a **Security Operations Center (SOC) Analyst**. This repository documents my progress through OverTheWire Bandit — a wargame designed to teach core Linux and security skills through real challenges.

Every writeup here goes beyond just "here's the answer." I explain the **why**, the **how**, and the **real-world relevance** of each technique — because understanding is more valuable than memorization.

---

## 🌐 What Is OverTheWire Bandit?

[OverTheWire Bandit](https://overthewire.org/wargames/bandit/) is a beginner-friendly wargame hosted at `bandit.labs.overthewire.org`. Players connect via **SSH** and must find a password hidden on the server to progress to the next level.

It is widely recommended as a starting point for learning:
- Linux command-line navigation
- File system permissions and ownership
- Text processing and searching
- SSH, networking, and basic security concepts

---

## 🛠 Skills Gained

| Skill | Description |
|---|---|
| 🐧 **Linux CLI** | Navigation, file management, redirection, piping |
| 🔑 **SSH** | Remote access, key-based auth, port forwarding |
| 🔒 **File Permissions** | Reading `rwx` bits, `chmod`, `chown`, SUID |
| 🔍 **Searching & Filtering** | `grep`, `find`, `strings`, `sort`, `uniq` |
| 🌐 **Networking Basics** | Ports, services, `nc`, `openssl`, connections |
| 📜 **Shell Scripting** | Loops, conditionals, text manipulation |
| 🧠 **Security Mindset** | Thinking like an attacker to defend better |

---

## 📊 Progress Tracker

| Level | Status | Key Concept |
|---|---|---|
| [Level 0](bandit/level0.md) | ✅ Complete | SSH connection basics |
| [Level 1](bandit/level1.md) | ✅ Complete | Reading files with special names |
| [Level 2](bandit/level2.md) | ✅ Complete | Filenames with spaces |
| [Level 3](bandit/level3.md) | ✅ Complete | Hidden files |
| [Level 4](bandit/level4.md) | ✅ Complete | Human-readable file detection |
| [Level 5](bandit/level5.md) | ✅ Complete | `find` with multiple conditions |
| [Level 6](bandit/level6.md) | ✅ Complete | Searching system-wide by owner/size |
| [Level 7](bandit/level7.md) | ✅ Complete | Searching inside file content with `grep` |
| [Level 8](bandit/level8.md) | ✅ Complete | Finding unique lines with `sort` + `uniq` |
| [Level 9](bandit/level9.md) | ✅ Complete | Extracting strings from binary files |
| [Level 10](bandit/level10.md) | ✅ Complete | Base64 encoding/decoding |
| Level 11–33 | 🔄 In Progress | — |

---

## 📁 Repository Structure

```
overthewire-security-writeups/
│
├── README.md               ← You are here
├── bandit/                 ← Level-by-level writeups
│   ├── level0.md
│   ├── level1.md
│   └── ...
├── notes/                  ← Reference notes and concepts
│   ├── linux-basics.md
│   ├── file-permissions.md
│   ├── networking-basics.md
│   └── useful-commands.md
├── resources.md            ← Curated learning resources
└── screenshots/            ← Optional visual walkthroughs
```

---

## 💼 Why This Project Matters for a SOC Analyst

A SOC Analyst is constantly working in Linux environments — investigating alerts, analyzing logs, and responding to incidents. The skills practiced here translate directly to real-world scenarios:

- **SSH analysis** → Identifying brute-force or unauthorized remote access in logs
- **File permissions** → Detecting privilege escalation or misconfigured access controls
- **Searching & filtering** → Parsing SIEM outputs, log files, and system data quickly
- **Networking** → Understanding port activity, service banners, and suspicious connections

This project demonstrates that I can **think critically, document clearly, and learn systematically** — key traits for any security professional.

---

## 📚 Notes & Resources

- 📝 [Linux Basics](notes/linux-basics.md)
- 🔒 [File Permissions](notes/file-permissions.md)
- 🌐 [Networking Basics](notes/networking-basics.md)
- ⚙️ [Useful Commands](notes/useful-commands.md)
- 📖 [Learning Resources](resources.md)

---

## 📬 Contact

Feel free to connect with me on [LinkedIn](https://linkedin.com) or open an issue if you spot an error in my writeups!

---

*"The more you sweat in training, the less you bleed in battle."*
