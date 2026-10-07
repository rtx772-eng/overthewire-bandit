# Bandit Level 11 → 12
**Date:** September 2026
**Assessor:** rtx772-eng
**Difficulty:** Easy-Medium

---

## 1. Executive Summary
This level required decoding a ROT13-encoded file using Linux's character translation tool `tr`. The encoding rotated every letter 13 positions in the alphabet — a classic obfuscation technique with no real security value, commonly encountered in CTFs and legacy systems.

---

## 2. Objective
Retrieve the password stored in `data.txt` where all lowercase and uppercase letters have been rotated by 13 positions (ROT13).

---

## 3. Environment
| Item | Details |
|------|---------|
| OS | Bandit Server (Linux) |
| Tools Used | cat, tr |
| Target File | ~/data.txt |
| User | bandit11 |

---

## 4. Methodology

### Step 1: Read the encoded file
```bash
cat data.txt
# Output: Gur cnffjbeq vf [encoded text]
# Observation: text is readable but meaningless — ROT13 encoded
```

### Step 2: Decode using tr
```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
# Output: The password is [password]
```
**Why this works:**
- `tr` replaces characters from list1 with matching characters from list2
- `A-Za-z` = full alphabet (all 26 letters, upper and lower)
- `N-ZA-Mn-za-m` = same alphabet shifted 13 positions
- ROT13 is self-reversing — encode and decode use identical command

---

## 5. Findings
| # | Finding | Severity | Description |
|---|---------|----------|-------------|
| 1 | Weak obfuscation | Low | ROT13 provides zero security — anyone knowing the pattern decodes instantly |

---

## 6. What Went Wrong
- Initially confused why `N-ZA-M` was used instead of just `N-Z`
- Realized `N-Z` is only 13 letters — needed `A-M` to complete the full 26-letter mapping

---

## 7. New Commands Learned
```bash
tr 'list1' 'list2'    # replace characters from list1 with list2
cat file | tr 'A-Za-z' 'N-ZA-Mn-za-m'  # ROT13 decode
```
**tr = translate** — mechanically replaces characters. It doesn't know about ROT13 — it just swaps characters based on the mapping you provide.

---

## 8. Red Team Significance
ROT13 and simple encoding schemes appear frequently in:
- CTF challenges as first-layer obfuscation
- Malware that encodes strings to evade basic signature detection
- Legacy systems storing "obfuscated" (not encrypted) data

**Key insight:** Encoding ≠ Encryption. ROT13 has no key — anyone who recognizes the pattern decodes it instantly. Real attackers use this to bypass naive string-matching defenses.

---

## 9. Lessons Learned
1. `tr` is a character replacement tool — not encryption-aware
2. ROT13 works because 26 ÷ 2 = 13 — shifting twice returns to original
3. Encoding and encryption are completely different concepts
4. `N-ZA-M` = alphabet cut in half and swapped to create the 13-position shift

---

## 10. Remediation
ROT13 should never be used to protect sensitive data. Use proper encryption (AES-256) for data at rest and TLS for data in transit.
