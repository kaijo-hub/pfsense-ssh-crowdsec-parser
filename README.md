# pfSense CrowdSec Collection Suite

This suite provides a complete and reliable detection pipeline for SSH authentication events on pfSense systems. It includes two pfSense-specific SSH parsers (RFC 3164 and RFC 5424) and eight SSH security scenarios. The suite is compatible with both pfSense CE and pfSense Plus and is designed to deliver precise, noise-resistant detection for the FreeBSD/OpenSSH environment used by pfSense.

The suite has been validated through an extensive multi-stage hubtest process covering RFC 3164, RFC 5424, and mixed-format log streams.

---

## Overview

- Full compatibility with:
  - pfSense CE 2.8.1 (FreeBSD 15, process name "sshd")
  - pfSense Plus 25.11.x (FreeBSD 16, process name "sshd-session")
- Two pfSense-specific SSH parsers:
  - sshd-logs-rfc3164
  - sshd-logs-rfc5424
- Eight pfSense-specific SSH detection scenarios
- Support for IPv4 and IPv6
- Normalized metadata (log_type, source_ip, target_user, service)
- Compatible with CrowdSec enrichers (GeoIP, Dateparse, etc.)
- Designed specifically for pfSense FreeBSD/OpenSSH message variants

---

## Installation

### Part 1 - Installing CrowdSec on pfSense

CrowdSec for pfSense is distributed as a pfSense package:

https://github.com/crowdsecurity/pfSense-pkg-crowdsec/releases

#### pfSense CE 2.8.1 (FreeBSD 15)

- Fully supported
- No workarounds required
- SSH logs use the process name "sshd"
- Both RFC 3164 and RFC 5424 formats are supported by this suite

#### pfSense Plus 25.11.x (FreeBSD 16 kernel)

pfSense Plus 25.11.x is based on FreeBSD 16, which is not yet officially supported by CrowdSec.

To install CrowdSec on pfSense Plus 25.11.x:

1. Install using the FreeBSD 15 compatibility flag:
   sh install-crowdsec.sh --freebsd 15

2. Apply the temporary workaround described here:
   https://github.com/crowdsecurity/pfSense-pkg-crowdsec/issues/121

After installation, restart CrowdSec on pfSense:

service crowdsec.sh restart

---

### Part 2 - Installing this CrowdSec Collection Suite

From the CrowdSec Hub (recommended once published):

cscli collections install kaijo/pfsense-crowdsec-collection-suite

From a local repository clone:

git clone https://github.com/kaijo-hub/pfsense-crowdsec-collection-suite
cscli collections install ./pfsense-crowdsec-collection-suite

Restart CrowdSec afterwards:

service crowdsec.sh restart

---

## Parser Architecture

pfSense can emit SSH logs in three formats:

- RFC 3164 only
- RFC 5424 only
- Mixed (common when local and remote syslog are combined)

Additionally, pfSense CE and pfSense Plus use different process names:

- pfSense CE: "sshd"
- pfSense Plus: "sshd-session"

To ensure full compatibility, this suite provides two dedicated parsers:

### sshd-logs-rfc3164

- Parses pfSense BSD-style SSH logs
- Supports both "sshd" (CE) and "sshd-session" (Plus)
- Covers all relevant FreeBSD/OpenSSH SSH message variants

### sshd-logs-rfc5424

- Parses pfSense structured syslog SSH logs (RFC 5424)
- Supports both "sshd" (CE) and "sshd-session" (Plus)
- Required when pfSense sends logs to remote syslog servers in RFC 5424 format

### Mixed-format compatibility

Both parsers operate correctly when pfSense emits both formats simultaneously, validated through mixed-format hubtests.

---

## Supported SSH Event Types

Both parsers recognize and classify:

- ssh_success - Successful authentication
- ssh_pam_auth_error - PAM authentication failures
- ssh_permission_denied - Permission denied for valid users
- ssh_invalid_user - Attempts with non-existent users
- ssh_closed - Normal connection closure
- ssh_timeout_before_auth - Timeouts before authentication
- ssh_disconnect - Disconnects during or after authentication
- ssh_connection_flood - High-frequency connection attempts
- ssh_conn_closed_scan - Connection-closed enumeration patterns

---

## Scenarios

The suite includes eight pfSense-specific SSH scenarios, each available in:

- RFC 3164 variant
- RFC 5424 variant
- Mixed-format variant

These scenarios detect:

- Fast brute-force attacks
- Slow brute-force attacks
- Invalid-user scans
- Connection-closed enumeration
- Connection floods
- Disconnect abuse
- Timeout-before-auth abuse
- Successful login after brute-force attempts

All scenarios rely on the normalized log_type values produced by the parsers.

---

## Test Coverage (Hubtest)

This suite has undergone a three-stage hubtest validation:

1. RFC 3164 test suite  
   All pfSense SSH scenarios and both parsers tested against pure RFC 3164 logs.

2. RFC 5424 test suite  
   All scenarios and both parsers tested against pure RFC 5424 logs.

3. Mixed-format test suite  
   All scenarios and both parsers tested against logs containing interleaved RFC 3164 and RFC 5424 entries.

### Test assets included

For each scenario and format:

- config.yaml
- parser.assert
- scenario.assert
- Realistic pfSense SSH log files (*.log)

Across all test suites:

- Thousands of assertions
- Zero mismatches
- Deterministic enrichment
- Full compatibility across CE, Plus, and all log formats

---

## Limitations

- Only pfSense native FreeBSD/OpenSSH SSH messages are supported
- RFC 5424 structured data elements are not parsed
- Custom syslog pipelines that modify log format are not supported
- External SSH daemons or modified FreeBSD builds are out of scope

---

## Future Extensions

This suite will later include:

- pfSense OpenVPN parser
- pfSense OpenVPN security scenarios

---

## Author

Author: Johann (kaijo)  
GitHub: https://github.com/kaijo-hub



