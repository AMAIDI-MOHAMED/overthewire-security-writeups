# Bandit Level 8 → Level 9

## 🎯 Goal

The password for the next level is stored in the file `data.txt` and is the only line of text that occurs exactly **once** (all other lines are duplicated).

**Connect with:** `ssh bandit8@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- How to sort file contents with `sort`
- How `uniq` detects duplicate lines (and its requirement for sorted input)
- How to chain commands with pipes to build a simple data analysis pipeline
- The `-u` flag of `uniq` to print only unique lines

---

## 🛠 Commands Used

```bash
# Sort the file, then find the unique line
sort data.txt | uniq -u
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — Understand the problem**

The file contains many lines, most of which are duplicated. We need the one line that appears exactly once. 

```bash
wc -l data.txt
```

There are thousands of lines. Reading manually isn't an option.

**Step 2 — Why we need `sort` first**

`uniq` only detects **consecutive** duplicate lines — it does not compare non-adjacent lines. If the same line appears 50 lines apart, `uniq` won't consider them duplicates.

So we must first `sort` the file (which groups identical lines together), then pipe to `uniq`:

```bash
sort data.txt | uniq -u
```

- `sort data.txt` — alphabetically sorts all lines, putting identical lines next to each other
- `| uniq -u` — from the sorted output, print only lines that appear **exactly once** (`-u` = unique)

The single output line is the password.

**Step 3 — Understanding `uniq` flags**

| Flag | Behavior |
|------|----------|
| `uniq` (no flag) | Remove consecutive duplicates (keep one copy) |
| `uniq -u` | Print only lines with NO duplicates |
| `uniq -d` | Print only lines that ARE duplicated |
| `uniq -c` | Prefix each line with its count |

To see the counts yourself:
```bash
sort data.txt | uniq -c | sort -n
```
The line with count `1` is the password.

---

## 🔐 Cybersecurity / SOC Relevance

The `sort | uniq` pipeline is a staple of log analysis in security operations. Common SOC use cases include:

- **Finding unique IPs** in an access log: `awk '{print $1}' access.log | sort | uniq -c | sort -rn`
- **Identifying rare events** — in threat hunting, unusual (low-frequency) events are often more suspicious than common ones
- **Deduplicating alert data** before importing into a SIEM
- **Counting occurrences** of error codes or user agents to spot anomalies

The combination of `sort`, `uniq`, and `awk` is the foundation of command-line log analysis without needing specialized tools.

---

## 💡 Key Takeaways

- `uniq` only works on **consecutive** duplicate lines — always `sort` first
- `uniq -u` outputs only lines that appear exactly once
- `uniq -c` shows the count of each line — useful for frequency analysis
- Piping (`|`) lets you build powerful one-liner analysis pipelines

---

## ❌ Common Mistakes

- **Using `uniq -u` without `sort`** — duplicates that are not adjacent won't be detected
- **Using `uniq` alone (without `-u`)** — this removes duplicates but still prints one copy of each, which doesn't help identify the unique line
- **Expecting `uniq` to work like a database GROUP BY** — it's simpler and requires sorted input
