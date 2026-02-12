# pfSense SSH CrowdSec Parser & Scenarios (kaijo/sshd-logs-pfsense)

**Version:** 0.1.3  
**Author:** kaijo

A pfSense optimized CrowdSec parser and scenario collection providing full SSH log coverage and security detection for FreeBSD/pfSense systems.

This project is designed specifically for pfSense 24.x and 25.x, which use FreeBSD OpenSSH logging.  
It includes:

- A complete pfSense SSH parser  
- Six pfSense specific SSH security scenarios  
- A CrowdSec collection bundling parser + scenarios  
- Full compatibility with CrowdSec 1.5 (FreeBSD build)

---

## Supported Log Types

The parser recognizes and normalizes all relevant pfSense SSH log formats:

- Accepted login  
- PAM authentication error  
- Permission denied  
- Timeout before authentication  
- Connection closed  
- Disconnect (with reason and reason_code)  
- sshguard (ignored intentionally)

This ensures **complete pfSense SSH coverage**.

---

## Features

### pfSense specific SSH parser
- Covers all pfSense/FreeBSD SSH log formats  
- Works with both `sshd` and `sshd-session`  
- Produces clean CrowdSec metadata (`evt.Meta.*`)  
- IPv4 and IPv6 support  
- No external dependencies  
- Fully tested against real pfSense logs  

### â Included SSH security scenarios

| Scenario | Purpose |
|---------|---------|
| `ssh-bruteforce-fast-pfsense` | Detects fast SSH bruteforce attacks |
| `ssh-bruteforce-slow-pfsense` | Detects slow, stealthy bruteforce attacks |
| `ssh-success-after-bruteforce-pfsense` | Detects successful login after multiple failures |
| `ssh-connection-flood-pfsense` | Detects excessive SSH connection attempts |
| `ssh-disconnect-abuse-pfsense` | Detects repeated disconnects with nonâ€‘zero reason codes |
| `ssh-conn-closed-scan-pfsense` | Detects repeated connectionâ€‘closed events (recon) |

All scenarios use **leaky buckets**, fully compatible with pfSenseâFreeBSD CrowdSec build.

### a Collection: `kaijo/pfsense-ssh`

This collection bundles:

- The pfSense SSH parser  
- All six SSH scenarios  
- Tags for pfSense, SSH, FreeBSD, security  

---

## Changes in Version 0.1.3

### Parser
- Fixed `PF_DISCONNECT` pattern to support pfSense format  
  Example:  
  `port 49695:11: Normal Shutdown`
- Removed `PF_SSHGUARD` (not required; logs excluded by filter)
- Fully tested against real pfSense logs

### Scenarios
- Added six pfSense optimized SSH scenarios  
- All scenarios validated with `crowdsec -t`  
- FreeBSD compatible leaky bucket logic  
- pfSense specific naming (`kaijo/...-pfsense`)

### Collection
- Added `kaijo/pfsense-ssh` collection  
- Bundles parser + all scenarios  
- Hubâready structure

---

## Installation

### Manual parser installation

cscli parsers install ./parsers/s01-parse/kaijo/sshd-logs-pfsense.yaml

Install full SSH security bundle (recommended)
cscli collections install ./collections/kaijo-pfsense-ssh.yaml

### Once published in the CrowdSec Hub

cscli collections install kaijo/pfsense-ssh

## Example pfSense SSH Logs
- Accepted keyboard-interactive/pam for admin from 192.168.1.10 port 54321 ssh2
- error: PAM: Authentication error for root from 192.168.1.10
- process_output: ssh_packet_write_poll: Connection from user admin 192.168.1.10 port 54321: Permission denied
- Timeout before authentication for connection from 192.168.1.10 to 192.168.1.1, pid = 12345
- Connection closed by 192.168.1.10 port 54321
- Received disconnect from 192.168.1.10 port 54321:11: Normal Shutdown

## Repository Structure

pfsense-ssh-crowdsec-parser/
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


## Future Extensions

This repository will later include:
- pfSense OpenVPN parser
- pfSense OpenVPN security scenarios
- pfSense firewall log parser

## Author: kaijo  

## GitHub: https://github.com/kaijo-hub


