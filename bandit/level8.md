# 🔐 Bandit Level 8

## 🎯 Goal

The password is stored in `data.txt` and is the only line of text that occurs **exactly once**.

- **Login:** `ssh bandit8@bandit.labs.overthewire.org -p 2220`
- **Password:** *(password found in Level 7)*

---

## 🧠 What I Learned

- How to sort file contents with `sort`
- How `uniq -u` filters for unique (non-repeated) lines
- Why `sort` is required before `uniq`

---

## 🛠 Commands Used

```bash
sort data.txt | uniq -u
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Understand the data**

```bash
wc -l data.txt
```

The file has many lines. Most lines appear multiple times — one appears only once.

**Step 2 — Sort the file**

`uniq` only detects *adjacent* duplicate lines. To find all duplicates across the file, we first sort it so identical lines are grouped together:

```bash
sort data.txt
```

**Step 3 — Filter for unique lines**

```bash
sort data.txt | uniq -u
```

- `sort data.txt` — sorts all lines alphabetically, grouping identical lines
- `uniq -u` — outputs only lines that appear **exactly once** (unique lines)

Output: the single unique line, which is the password for Level 9.

---

## 🔐 Cybersecurity / SOC Relevance

`sort` + `uniq` is a classic pattern in log analysis:

- **Find unusual events**: In a log file full of routine entries, identify the single anomalous event
- **Count occurrences**: `sort | uniq -c | sort -rn` — count how often each line appears, sorted by frequency (useful for identifying top offenders, e.g., most active IPs)
- **De-duplicate IOC lists**: When merging threat intelligence feeds, remove duplicate IP addresses or hashes

Example: Find the top 5 most common source IPs in an access log:
```bash
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -5
```

---

## 💡 Key Takeaways

- `sort` is required before `uniq` because `uniq` only compares adjacent lines
- `uniq -u` = only unique lines (appear once)
- `uniq -d` = only duplicate lines (appear more than once)
- `uniq -c` = prefix each line with its count of occurrences

---

## ❌ Common Mistakes

- Running `uniq -u data.txt` without sorting first — misses non-adjacent duplicates
- Confusing `uniq -u` (unique only) with `uniq` (remove consecutive duplicates, keeping one copy)
