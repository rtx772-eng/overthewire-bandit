# Bandit Levels 14 & 15

## Level 14 → 15 ✅
**Goal:** Submit bandit14's password to port 30000 on localhost

### New Concept: Ports
```
IP address = building address (gets you to the right machine)
Port       = apartment number (gets you to the right service)

Port 22    = SSH
Port 80    = Web (HTTP)
Port 443   = Secure Web (HTTPS)
Port 30000 = Bandit challenge service
```
localhost = 127.0.0.1 = your own machine talking to itself

### New Command: nc (netcat)
```bash
nc localhost 30000
```
**What it does:** Opens a raw connection to a host on a specific port.
Sends and receives data directly — no encryption.

**How it works:**
1. You connect with `nc host port`
2. Server is silent — it waits for YOUR input
3. You type data + Enter
4. Server responds

**Analogy:** A telephone call with no greeting — they pick up and wait for you to speak.

### Process:
```bash
cat /etc/bandit_pass/bandit14   # get current password
nc localhost 30000               # connect to port 30000
[type password + Enter]          # send it
[receive next password]          # done ✅
```

### What I Learned:
- Servers often wait silently for input — don't panic, just type
- Ports direct traffic to the right service on a machine
- nc sends data in plain text — no encryption

---

## Level 15 → 16 ✅
**Goal:** Same as Level 14 but using SSL/TLS encryption on port 30001

### New Concept: SSL/TLS Encryption
```
nc (plain)        = postcard — anyone can read it
openssl s_client  = sealed envelope — only sender and receiver see it
```
SSL/TLS creates an encrypted tunnel so nobody can intercept your data.

### New Command: openssl s_client
```bash
openssl s_client -connect localhost:30001
```
**What it does:** Same as nc but with SSL/TLS encryption.
Note: host:port combined with `:` (not a space like nc)

**SSL Handshake output is normal** — all that text before the prompt is the encryption being set up. Wait for `read R BLOCK` then type your password.

### Process:
```bash
openssl s_client -connect localhost:30001
[wait for read R BLOCK]
[type password + Enter]
[receive next password] ✅
```

### nc vs openssl s_client:
| | nc | openssl s_client |
|--|--|--|
| Encryption | ❌ None | ✅ SSL/TLS |
| Usage | `nc host port` | `openssl s_client -connect host:port` |
| When to use | Internal/lab testing | Any real-world secure service |

### What I Learned:
- Always prefer encrypted connections in real scenarios
- SSL/TLS handshake = normal — just the encryption being established
- host:port format used by openssl (different from nc's space-separated format)
- In real red teaming: many services use SSL — openssl s_client lets you interact with them manually

### Red Team Significance:
```
nc uses:
- Testing open ports
- Creating reverse shells
- Transferring files between machines
- Setting up listeners

openssl s_client uses:
- Testing HTTPS services manually
- Inspecting SSL certificates
- Interacting with encrypted services
- Certificate information gathering
```
