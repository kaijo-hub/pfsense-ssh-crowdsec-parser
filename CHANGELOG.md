# Changelog

## 0.1.4 — 2026-02-14
### Added
- Added full Hub test suite:
  - Parser test for `kaijo/sshd-logs-pfsense`
  - Six scenario test suites with sample logs and expected alerts
- Added pfSense SSH log samples for Hub validation

### Improved
- Updated collection structure to match CrowdSec Hub requirements
- Removed unsupported `version:` field from collection file
- Updated documentation and metadata for Hub submission
- Improved parser metadata and descriptions

### Fixed
- Corrected parser config to remove obsolete fields
- Ensured all log_type values are used by at least one scenario

---

## 0.1.3 — 2026-02-12
### Added
- Added six pfSense‑optimized SSH scenarios:
  - ssh-bruteforce-fast-pfsense
  - ssh-bruteforce-slow-pfsense
  - ssh-success-after-bruteforce-pfsense
  - ssh-connection-flood-pfsense
  - ssh-disconnect-abuse-pfsense
  - ssh-conn-closed-scan-pfsense
- Added collection `kaijo/pfsense-ssh`
- Added tags for pfSense, SSH, FreeBSD, security

### Improved
- Parser documentation updated
- Repository structure aligned with CrowdSec Hub standards

---

## 0.1.2
### Fixed
- Minor parser adjustments
- Improved log_type normalization

---

## 0.1.0
### Initial Release
- Initial pfSense SSH parser

