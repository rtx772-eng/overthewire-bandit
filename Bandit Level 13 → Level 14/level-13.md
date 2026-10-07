# Bandit Level 13 → 14
**Date:** September 2026
**Assessor:** rtx772-eng
**Difficulty:** Medium

---

## 1. Executive Summary
This level introduced SSH key-based authentication as an alternative to password authentication. Instead of a password, a private key file was used to log into the next level. The assessment also required transferring the key file between machines using SCP, highlighting the risks of exposed private keys in real environments.

---

## 2. Objective
Use a private SSH key found in the home directory to log in as bandit14 — no password provided.

---

## 3. Environment
| Item | Details |
|------|---------|
| OS | Kali Linux (local) + Bandit Server (remote) |
| Tools Used | ssh, scp, chmod |
| Target | bandit14@bandit.labs.overthewire.org:2220 |
| User | bandit13 |

---

## 4. Methodology

### Step 1: Identify the key file
```bash
ls -lah
# Found: sshkey.private (owned by bandit14)
```

### Step 2: Copy key to local Kali machine
```bash
# Run from Kali terminal:
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
```
**Why:** Cannot SSH from inside Bandit back to itself (localhost blocked). Must connect from local machine.

### Step 3: Fix key permissions
```bash
chmod 400 sshkey.private
```
**Why:** SSH refuses to use private keys readable by group or others. 400 = owner read only.

### Step 4: Connect as bandit14 using the key
```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```
**Result:** Logged in as bandit14 with no password required ✅

---

## 5. Findings
| # | Finding | Severity | Description |
|---|---------|----------|-------------|
| 1 | Exposed private key | Critical | Private key readable by bandit13 — anyone with access to bandit13 can log in as bandit14 |
| 2 | No password required | High | Key-only auth means stolen key = full access |

---

## 6. What Went Wrong
- Tried to SSH from inside Bandit server — blocked (localhost restriction)
- Used wrong port flag: `-P` (scp) vs `-p` (ssh) — caused connection failure
- Typed `bandit14@@` instead of `bandit14@` — typo caused auth failure

---

## 7. New Commands Learned
```bash
scp -P 2220 user@host:remotefile .    # copy file FROM remote TO local
ssh -i keyfile user@host -p 2220      # SSH with private key
chmod 400 sshkey.private              # fix key permissions
```

**Important difference:**
```
ssh  → lowercase -p for port
scp  → UPPERCASE -P for port
```

**Remote file format for scp:**
```
user@host:filepath
```

---

## 8. Red Team Significance
Private SSH keys are among the most valuable finds during a penetration test:
- **No password needed** — key file alone grants full access
- **Lateral movement** — one compromised machine's key may unlock others
- **Persistence** — attacker adds their public key to target's authorized_keys
- **Common finding** — developers often leave private keys in home directories, repos, or backup files

**Real world example:** Many major breaches involved stolen SSH keys found in exposed GitHub repositories or unsecured servers. A single exposed key can compromise entire infrastructure.

**Attacker mindset:** During post-exploitation, always search for:
```bash
find / -name "*.pem" 2>/dev/null
find / -name "id_rsa" 2>/dev/null
find / -name "*.key" 2>/dev/null
find ~/.ssh/ -type f 2>/dev/null
```

---

## 9. Lessons Learned
1. SSH key authentication = more secure than passwords but key exposure = critical risk
2. Private key = never share, never leave exposed
3. SSH enforces strict permissions — chmod 400 is required
4. scp uses UPPERCASE -P, ssh uses lowercase -p
5. Remote file format: user@host:filepath (colon separates machine from path)
6. localhost SSH blocking = common security control on shared servers

---

## 10. Remediation
1. Never store private keys with world-readable permissions
2. Rotate SSH keys regularly
3. Use SSH certificates instead of static keys for large environments
4. Monitor for unauthorized key additions to authorized_keys files
5. Scan repositories and servers for accidentally exposed private keys
