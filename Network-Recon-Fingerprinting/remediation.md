# Remediation & Hardening

## 1. Network Perimeter Defense

- **Action:** Implement a firewall (like `ufw` or `iptables`) and configure it to only allow connections to port 22 from specific, trusted IP addresses (e.g., the administrator's VPN).
- **Action:** Deploy an Intrusion Detection System (IDS) or a tool like `PortSentry` to automatically block IP addresses that perform port scans.

## 2. SSH Hardening

- **Action:** Disable password authentication and enforce SSH keys.
- **Action:** Disable root login over SSH (`PermitRootLogin no`).
- **Action:** If possible, configure the SSH daemon to not broadcast its exact version number to make fingerprinting harder.

## 3. Continuous Monitoring

- **Action:** Forward firewall logs and authentication logs to a SIEM to alert on multiple connection attempts from a single source IP.
