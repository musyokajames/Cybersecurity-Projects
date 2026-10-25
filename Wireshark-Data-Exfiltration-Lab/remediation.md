# Remediation & Hardening

## 1. Data Loss Prevention (DLP)

- **Action:** Implement a DLP solution to inspect outbound traffic. If a system attempts to send unencrypted data patterns (like SSNs or credit cards) to an external IP, block it and alert the SOC.

## 2. Network Segmentation

- **Action:** Restrict outbound traffic. Servers and workstations should only be allowed to communicate on specific ports to specific IP addresses, preventing arbitrary data exfiltration to unknown hosts.

## 3. Endpoint Detection and Response (EDR)

- **Action:** Deploy EDR agents to monitor process execution. If a script like Python or Bash attempts to open a network connection and blast thousands of lines of data, the EDR should terminate the process.
