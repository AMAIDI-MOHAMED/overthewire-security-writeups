# Bandit Level 11 → Level 12

## 🎯 Goal

The password for the next level is stored in the file `data.txt`, where all lowercase (`a-z`) and uppercase (`A-Z`) letters have been rotated by 13 positions (ROT13).

**Connect with:** `ssh bandit11@bandit.labs.overthewire.org -p 2220`

---

## 🧠 What I Learned

- What the ROT13 cipher is and how it works
- How to use the `tr` (translate) command to perform character substitution
- Why simple substitution ciphers offer no real security
- The historical context of ROT13 and Caesar ciphers

---

## 🛠 Commands Used

```bash
# View the encoded content
cat data.txt

# Decode ROT13 using tr
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

---

## 🔍 Step-by-Step Explanation

**Step 1 — View the file**

```bash
cat data.txt
```

Output (example):
```
Gur cnffjbeq vf <fbzrguvat>
```

This is readable English shifted by 13 characters — ROT13.

**Step 2 — Understand ROT13**

ROT13 ("rotate by 13") is a simple letter substitution cipher that replaces each letter with the letter 13 positions after it in the alphabet. Because the alphabet has 26 letters, applying ROT13 twice returns the original text — it's its own inverse.

```
A → N    N → A
B → O    O → B
...
M → Z    Z → M
```

**Step 3 — Decode with `tr`**

The `tr` command translates (substitutes) characters. We provide two character sets:
- Input set: `A-Za-z` (all letters)
- Output set: `N-ZA-Mn-za-m` (letters shifted by 13)

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Breaking down `N-ZA-Mn-za-m`:
- `N-Z` — maps A-M to N-Z
- `A-M` — maps N-Z to A-M
- `n-z` — same for lowercase a-m
- `a-m` — same for lowercase n-z

Output:
```
The password is <the_actual_password>
```

---

## 🔐 Cybersecurity / SOC Relevance

ROT13 and Caesar ciphers are examples of substitution ciphers — historically significant but completely insecure by modern standards. They are frequency-analysable (common letters like 'e', 't', 'a' remain common after rotation) and brute-forceable in seconds.

In modern security contexts, you may encounter ROT13 in:
- **CTF challenges** — it's a classic beginner encoding
- **Obfuscated scripts** — occasionally used in malware to hide strings from basic string scanners
- **Spam filters evasion** — simple encoding to bypass keyword filters

The `tr` command itself is powerful beyond ROT13: it can delete characters (`tr -d`), squeeze repeats (`tr -s`), and complement sets (`tr -c`). Security scripts often use `tr` for data normalization and cleaning.

---

## 💡 Key Takeaways

- ROT13 is a reversible character substitution (applying it twice restores the original)
- `tr 'from-set' 'to-set'` performs character-by-character translation
- Simple ciphers like ROT13, Caesar, and Vigenère are **not** encryption — they're obfuscation
- Real encryption (AES, RSA) involves mathematical operations that can't be reversed without a key

---

## ❌ Common Mistakes

- **Treating ROT13 as encryption** — it provides zero security; anyone can decode it instantly
- **Getting the `tr` character sets wrong** — the order matters; `N-ZA-M` maps A-M→N-Z and N-Z→A-M
- **Forgetting that ROT13 is self-inverting** — the same command encodes and decodes
