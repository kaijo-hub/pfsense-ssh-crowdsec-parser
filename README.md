# kaijo/pfsense-ssh - pfSense SSH Collection for CrowdSec

This collection provides full SSH log parsing and attack detection for **pfSense 24.x / 25.x** systems.  
pfSense uses a FreeBSD-specific OpenSSH logging format that differs significantly from Linux, causing the default CrowdSec SSH parser to miss or misclassify events.

This collection adds:

- A pfSense-optimized SSH parser  
- Six pfSense specific SSH scenarios  
- A complete Hub test suite  
- Full support for pfSense/FreeBSD OpenSSH logs  
- Accurate detection of bruteforce, slow bruteforce, connection floods, disconnect abuse, and post-bruteforce success

---

## Why this collection is needed

pfSense uses a FreeBSD-patched OpenSSH implementation that produces log lines such as:

- error: PAM: Authentication error for admin from 1.2.3.4
- Connection closed by authenticating user admin 1.2.3.4 port 22 [preauth]
- Received disconnect from 5.6.7.8 port 54321:11: disconnected by user

These formats:  

- do **not** match Linux OpenSSH logs   
- are **not** parsed by the default CrowdSec SSH parser   
- cause SSH scenarios to never trigger on pfSense systems   

This collection provides **full coverage** for all pfSense SSH log variants.

---

## Supported Log Types

The parser recognizes and normalizes all relevant pfSense SSH log formats:

- Accepted login  
- PAM authentication error  
- Permission denied  
- Timeout before authentication  
- Connection closed  
- Disconnect (with reason and reason_code)  


This ensures **complete pfSense SSH coverage**.

---

## Features

### pfSense-specific SSH parser

- `kaijo/sshd-logs-pfsense` 
  Parses all pfSense/FreeBSD SSH log formats and normalizes them into CrowdSec compatible `log_type` values.

Key characteristics:

- Covers all pfSense/FreeBSD SSH log formats  
- Works with both `sshd` and `sshd-session`  
- Produces clean CrowdSec metadata (`evt.Meta.*`)  
- IPv4 and IPv6 support  
- No external dependencies  
- Fully tested against real pfSense logs  

###  Included SSH security scenarios

| Scenario | Purpose |
|---------|---------|
| `ssh-bruteforce-fast-pfsense` | Detects fast SSH bruteforce attacks |
| `ssh-bruteforce-slow-pfsense` | Detects slow, stealthy bruteforce attacks |
| `ssh-success-after-bruteforce-pfsense` | Detects successful login after multiple failures |
| `ssh-connection-flood-pfsense` | Detects excessive SSH connection attempts |
| `ssh-disconnect-abuse-pfsense` | Detects repeated disconnects with non-zero reason codes |
| `ssh-conn-closed-scan-pfsense` | Detects repeated connection-closed events (recon) |

All scenarios use **leaky buckets**, fully compatible with pfSense FreeBSD/CrowdSec build
Each scenario is tuned specifically for pfSense SSH behavior.

---

## Collection: `kaijo/pfsense-ssh`
 
This collection bundles:

- The pfSense SSH parser
- All six SSH scenarios
- Tags for pfSense, SSH, FreeBSD, security

---

## Hub Test Suite

This collection includes a complete Hub compatible test suite:

tests/parsers/kaijo/sshd-logs-pfsense/logs/sample.log
tests/parsers/kaijo/sshd-logs-pfsense/expected.json

tests/scenarios/kaijo/<scenario-name>/logs/sample.log
tests/scenarios/kaijo/<scenario-name>/expected.json

These tests validate:

- parser correctness
- scenario triggering
- alert generation
- pfSense-specific log coverage

---

## Installation

### Once published in the CrowdSec Hub

cscli collections install kaijo/pfsense-ssh

### Manual installation (development/testing)

cscli parsers install ./parsers/s01-parse/kaijo/sshd-logs-pfsense.yaml
cscli collections install ./collections/kaijo-pfsense-ssh.yaml

---

## Example pfSense SSH Logs
- Accepted keyboard-interactive/pam for admin from 192.168.1.10 port 54321 ssh2
- error: PAM: Authentication error for root from 192.168.1.10
- process_output: ssh_packet_write_poll: Connection from user admin 192.168.1.10 port 54321: Permission denied
- Timeout before authentication for connection from 192.168.1.10 to 192.168.1.1, pid = 12345
- Connection closed by 192.168.1.10 port 54321
- Received disconnect from 192.168.1.10 port 54321:11: Normal Shutdown

---

## Repository Structure

kaijo-pfsense-collection/
│
├── parsers/
│   └── s01-parse/
│       └── kaijo/
│           └── sshd-logs-pfsense.yaml
│
├── scenarios/
│   └── kaijo/
│       ├── ssh-bruteforce-fast-pfsense.yaml
│       ├── ssh-bruteforce-slow-pfsense.yaml
│       ├── ssh-success-after-bruteforce-pfsense.yaml
│       ├── ssh-connection-flood-pfsense.yaml
│       ├── ssh-disconnect-abuse-pfsense.yaml
│       └── ssh-conn-closed-scan-pfsense.yaml
│
└── collections/
    └── kaijo-pfsense-ssh.yaml

---

## Future Extensions

This repository will later include:
- pfSense OpenVPN parser
- pfSense OpenVPN security scenarios

---

## Author: kaijo  

## GitHub: https://github.com/kaijo-hub


