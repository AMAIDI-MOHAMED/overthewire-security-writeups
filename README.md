# 🔐 OverTheWire Bandit — Security Writeups & Portfolio

> A structured collection of writeups, notes, and lessons from solving the OverTheWire Bandit wargame, built as a cybersecurity portfolio for aspiring SOC Analysts.

---

## 👋 About Me

I'm a cybersecurity learner actively building hands-on skills to pursue a career as a **SOC Analyst**. This repository documents my journey through the **OverTheWire Bandit** wargame — one of the best beginner-friendly platforms for developing Linux command-line and security fundamentals.

Each writeup is written with a focus on **understanding**, not just commands — because a good SOC Analyst knows *why* something works, not just *how* to run it.

---

## 🌐 What is OverTheWire Bandit?

[OverTheWire Bandit](https://overthewire.org/wargames/bandit/) is a free, browser-accessible wargame designed to teach the fundamentals of Linux and security through increasingly challenging levels. Each level requires you to find a password stored somewhere on the system using Linux commands and creative problem-solving.

It is widely recommended as a **first step** in cybersecurity training because it builds:
- Comfort with the Linux terminal
- Logical and methodical problem-solving
- An instinct for how systems store and protect data

---

## 🛠 Skills Developed

| Skill | Description |
|-------|-------------|
| 🐧 Linux Command Line | Navigation, file operations, piping, redirection |
| 🔑 SSH & Remote Access | Connecting to remote systems securely |
| 📁 File Permissions | Understanding `rwx`, `chmod`, `chown`, special bits |
| 🔍 Searching & Discovery | `find`, `grep`, `strings`, `file` commands |
| 🌐 Basic Networking | Ports, TCP connections, netcat, SSL/TLS |
| 🧩 Problem Solving | Reading man pages, thinking creatively under constraints |
| 📝 Documentation | Writing clear technical writeups (a critical SOC skill) |

---

## 📊 Progress Tracker

| Level | Status | Key Concept |
|-------|--------|-------------|
| [Bandit 0](bandit/level0.md) | ✅ Completed | SSH login basics |
| [Bandit 1](bandit/level1.md) | ✅ Completed | Reading files with special names |
| [Bandit 2](bandit/level2.md) | ✅ Completed | Files with spaces in the name |
| [Bandit 3](bandit/level3.md) | ✅ Completed | Hidden files |
| [Bandit 4](bandit/level4.md) | ✅ Completed | Human-readable files |
| [Bandit 5](bandit/level5.md) | ✅ Completed | Finding files by properties |
| [Bandit 6](bandit/level6.md) | ✅ Completed | Searching the entire filesystem |
| [Bandit 7](bandit/level7.md) | ✅ Completed | Searching within file content |
| [Bandit 8](bandit/level8.md) | ✅ Completed | Finding unique lines |
| [Bandit 9](bandit/level9.md) | ✅ Completed | Human-readable strings in binary |
| [Bandit 10](bandit/level10.md) | ✅ Completed | Base64 encoding/decoding |
| [Bandit 11](bandit/level11.md) | ✅ Completed | ROT13 cipher |
| [Bandit 12](bandit/level12.md) | ✅ Completed | Hex dumps and file compression |
| [Bandit 13](bandit/level13.md) | ✅ Completed | SSH key authentication |
| [Bandit 14](bandit/level14.md) | ✅ Completed | Connecting to local services |
| [Bandit 15](bandit/level15.md) | ✅ Completed | SSL/TLS connections |
| Bandit 16+ | 🔄 In Progress | Port scanning, encryption |

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
│
├── notes/                  ← Topic-based reference notes
│   ├── linux-basics.md
│   ├── file-permissions.md
│   ├── networking-basics.md
│   └── useful-commands.md
│
├── resources.md            ← Curated learning resources
└── screenshots/            ← Visual references (optional)
```

---

## 🎯 Why This Project Matters for SOC Analysts

A Security Operations Center (SOC) Analyst spends a large portion of their day:
- Investigating suspicious activity **on Linux-based systems**
- Searching log files and identifying anomalies
- Understanding file permissions and access control
- Using SSH to connect to servers for analysis
- Reading and understanding network traffic

**Every skill practiced in Bandit is directly applicable to real-world SOC work.** The habit of documenting findings clearly — as done in this repo — mirrors the incident reporting and runbook documentation that SOC teams rely on daily.

---

## 📚 Notes

Supplementary notes are available in the [`notes/`](notes/) folder, covering:
- [Linux Basics](notes/linux-basics.md)
- [File Permissions](notes/file-permissions.md)
- [Networking Basics](notes/networking-basics.md)
- [Useful Commands Reference](notes/useful-commands.md)

---

## 📖 Resources

See [`resources.md`](resources.md) for a curated list of cybersecurity learning platforms, Linux tutorials, and tools.

---

## 🤝 Connect

This repository is part of my active cybersecurity portfolio. If you're a recruiter, fellow learner, or security professional — feel free to explore, fork, or reach out!

> *"The more you sweat in training, the less you bleed in battle."*
