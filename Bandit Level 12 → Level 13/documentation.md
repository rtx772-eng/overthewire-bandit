# Bandit Level 12 → 13
**Date:** September 2026
**Assessor:** rtx772-eng
**Difficulty:** Hard

---

## 1. Executive Summary
This level presented a hexdump of a file compressed multiple times using gzip, bzip2, and tar. Successful extraction required converting the hexdump back to binary then methodically identifying and decompressing each layer. The core skill developed was reading file types from content rather than filename — critical for forensic analysis and malware investigation.

---

## 2. Objective
Retrieve the password from `data.txt` — a hexdump of a repeatedly compressed file.

---

## 3. Environment
| Item | Details |
|------|---------|
| OS | Bandit Server (Linux) |
| Tools Used | mktemp, cp, xxd, file, mv, gzip, bzip2, tar, cat |
| Target File | ~/data.txt |
| User | bandit12 |

---

## 4. Methodology

### Step 1: Create safe workspace
```bash
mktemp -d
# Output: /tmp/tmp.ABC123
cd /tmp/tmp.ABC123
```
**Why:** Working in /tmp/ prevents cluttering home directory. mktemp -d creates a unique temporary directory.

### Step 2: Copy file to workspace
```bash
cp /home/bandit12/data.txt .
```

### Step 3: Convert hexdump to binary
```bash
xxd -r data.txt > file.bin
```
**Why:** data.txt contains hex-encoded binary data. xxd -r reverses this back to the actual binary file.

### Step 4: Decompression cycle (repeated 7+ times)
```bash
file file.bin          # identify compression type
mv file.bin file.gz    # rename with correct extension
gzip -d file.gz        # decompress
file file              # identify next layer
mv file file.bz2
bzip2 -d file.bz2
file file
mv file file.gz
gzip -d file.gz
file file
mv file file.tar
tar -xf file.tar       # produces data5.bin
file data5.bin
mv data5.bin data5.tar
tar -xf data5.tar      # produces data6.bin
file data6.bin
mv data6.bin data6.bz2
bzip2 -d data6.bz2
file data6
mv data6 data6.tar
tar -xf data6.tar      # produces data8.bin
file data8.bin
mv data8.bin data8.gz
gzip -d data8.gz
file data8             # ASCII text ✅
cat data8              # PASSWORD
```

---

## 5. Findings
| # | Finding | Severity | Description |
|---|---------|----------|-------------|
| 1 | Data obfuscation via compression | Info | Multiple compression layers used to obscure data |
| 2 | Hexdump encoding | Info | Binary stored as hexdump to enable text transmission |

---

## 6. What Went Wrong
- Initially tried to work in home directory — realized need separate workspace
- Forgot mktemp creates a FILE without -d flag — can't cd into a file
- Confused when new filenames (data5, data6, data8) appeared after tar extraction — realized the cycle never changes regardless of filename
- Attempted wrong decompression tool before checking with `file` command

---

## 7. New Commands Learned
```bash
mktemp -d              # create temp directory in /tmp/
cp source dest         # copy file
mv old new             # rename or move file
xxd -r hex > binary    # reverse hexdump to binary
file filename          # identify file type from content
gzip -d file.gz        # decompress gzip
bzip2 -d file.bz2      # decompress bzip2
tar -xf file.tar       # extract tar archive
```

**The Decompression Cycle:**
```
file → identify → rename → decompress → repeat
```

---

## 8. Red Team Significance
Multi-layer compression and encoding is used in:
- **Malware packing** — malware authors compress/encode payloads to evade antivirus detection
- **Data exfiltration** — compressing stolen data before transmission reduces size and may bypass DLP tools
- **Forensic analysis** — investigators must unpack multiple layers to find evidence
- **Steganography** — hiding data inside other files

**Key insight:** Never trust a filename. The `file` command reads actual file content (magic bytes) — a renamed `.exe` is still an executable. This is fundamental to malware analysis.

---

## 9. Lessons Learned
1. **"The filename means nothing. The `file` command tells the truth."**
2. The decompression cycle never changes — identify, rename, decompress, repeat
3. mktemp -d creates directory (-d flag critical)
4. xxd -r reverses hexdump back to usable binary
5. tar -xf requires the -f flag — without it, refuses to read from file
6. Reading error messages carefully reveals the fix every time

---

## 10. Remediation
In production systems:
- Implement file type validation based on content not extension
- Deploy antivirus/EDR solutions that unpack compressed files before scanning
- Monitor for unusual compression tool usage (gzip/bzip2/tar in sequence)
