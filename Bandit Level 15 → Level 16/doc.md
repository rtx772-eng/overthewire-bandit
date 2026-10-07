# Bandit Level 15 → 16
**Date:** September 2026
**Assessor:** rtx772-eng
**Difficulty:** Easy-Medium

---

## 1. Executive Summary
This level built on Level 14 by requiring SSL/TLS encrypted communication instead of plaintext. Using openssl s_client, credentials were submitted securely to port 30001. The contrast with Level 14 directly demonstrates the security improvement encryption provides and introduces certificate inspection — a valuable reconnaissance technique.

---

## 2. Objective
Retrieve the next password by submitting the current level's password to port 30001 on localhost using SSL/TLS encryption.

---

## 3. Environment
| Item | Details |
|------|---------|
| OS | Bandit Server (Linux) |
| Tools Used | openssl s_client |
| Target | localhost:30001 |
| User | bandit15 |

---

## 4. Methodology

### Step 1: Connect using SSL/TLS
```bash
openssl s_client -connect localhost:30001
```
**Why:** Regular nc cannot handle SSL/TLS encryption. openssl s_client establishes an encrypted connection.

### Step 2: Wait for SSL handshake to complete
```
[Large SSL handshake output]
...
read R BLOCK   ← server ready, waiting for input
```
**Why:** SSL handshake output is normal — encryption being established. Wait for `read R BLOCK`.

### Step 3: Submit credentials
```
[typed bandit15 password] + Enter
```

### Step 4: Receive response
```
Correct!
[bandit16 password returned] ✅
```

---

## 5. Findings
| # | Finding | Severity | Description |
|---|---------|----------|-------------|
| 1 | Self-signed certificate | Medium | Certificate not issued by trusted CA — potential MITM risk in real environments |
| 2 | TLSv1.3 in use | Info | Modern TLS version — good security practice |

---

## 6. What Went Wrong
- Initially confused by large SSL handshake output — thought something went wrong
- Learned: all that output is normal — just wait for `read R BLOCK`

---

## 7. New Commands Learned
```bash
openssl s_client -connect host:port    # connect with SSL/TLS encryption
openssl s_client -connect localhost:30001
```

**Key difference from nc:**
```
nc host port                          → plaintext
openssl s_client -connect host:port   → encrypted SSL/TLS
```

**SSL Handshake output explained:**
```
CONNECTED              = connection successful
Certificate chain      = server's identity proof
TLSv1.3               = encryption version
self-signed certificate = no trusted CA (normal in labs)
read R BLOCK           = ready — type your input now
```

---

## 8. Red Team Significance
SSL/TLS inspection is critical in red teaming:

**Certificate reconnaissance:**
```bash
openssl s_client -connect target.com:443
# Reveals: certificate expiry, issuer, domain names (SANs)
# Expired certs = misconfiguration = potential vulnerability
# SANs reveal other domains/subdomains on same server
```

**Finding weak SSL configurations:**
- Old TLS versions (TLS 1.0, 1.1) = vulnerable to known attacks
- Weak cipher suites = potential decryption
- Self-signed certificates = potential MITM attack vector
- Certificate mismatches = potential phishing indicator

**Real world application:**
Red teamers use openssl s_client during reconnaissance to:
1. Map subdomains from certificate SANs
2. Identify certificate expiry for social engineering timing
3. Detect misconfigured SSL (weak ciphers, old versions)
4. Manually test HTTPS APIs and services

---

## 9. Lessons Learned
1. SSL/TLS handshake output is normal — don't panic, wait for `read R BLOCK`
2. openssl s_client uses `host:port` format (colon, not space like nc)
3. Self-signed certificates are common in labs — warning is expected
4. Encryption protects data in transit — plaintext services expose everything
5. Always prefer encrypted connections in real environments

---

## 10. Remediation
1. Use certificates from trusted Certificate Authorities (not self-signed) in production
2. Enforce TLS 1.2 minimum — disable TLS 1.0 and 1.1
3. Configure strong cipher suites only
4. Implement HSTS (HTTP Strict Transport Security)
5. Monitor certificate expiry and renew before expiration
