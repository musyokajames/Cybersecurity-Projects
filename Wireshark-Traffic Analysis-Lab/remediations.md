# Remediation & Hardening

## 1. Mitigate Network Scanning (T1046)

- **Action:** Implement an Intrusion Detection System (IDS) like Snort or Suricata to alert on rapid connection attempts to multiple ports.
- **Action:** Use a host-based firewall (like `ufw`) to drop traffic on unused ports.

## 2. Prevent Credential Theft (T1040)

- **Action:** Enforce encryption everywhere. Replace HTTP with HTTPS, Telnet with SSH, and unencrypted databases with TLS-encrypted connections.
- **Action:** Segment the network. Ensure that an attacker who compromises one VLAN cannot sniff the traffic of another.
