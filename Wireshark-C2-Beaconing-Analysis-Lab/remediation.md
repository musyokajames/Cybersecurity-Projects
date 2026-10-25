# Remediation & Hardening

## 1. Egress Filtering (Firewall Rules)

- **Action:** Implement strict egress (outbound) firewall rules. Workstations should not be allowed to communicate directly with the internet on arbitrary ports. Only allow necessary traffic (e.g., HTTP/HTTPS via a proxy).

## 2. Network Detection and Response (NDR)

- **Action:** Deploy NDR tools or an Intrusion Detection System (IDS) that can baseline traffic patterns and alert on consistent, rhythmic beaconing activity.

## 3. Endpoint Detection and Response (EDR)

- **Action:** Use EDR to monitor for unexpected processes (like a script or unknown binary) initiating outbound network connections at regular intervals.
