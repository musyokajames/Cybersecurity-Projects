# Remediation & Hardening

## 1. Enforce HTTPS (T1040)

- **Action:** Configure web browsers via Group Policy (or enterprise settings) to block access to HTTP-only websites.
- **Action:** Use browser extensions like "HTTPS Everywhere" to force encrypted connections where available.

## 2. Monitor DNS Traffic

- **Action:** Implement DNS filtering (like Cisco Umbrella or Pi-hole) to block requests to known malicious domains.
- **Action:** Monitor DNS logs for unusually high volumes of requests (DNS Tunneling) or requests to newly registered domains (NRDs).

## 3. Detect Scripted Web Requests (T1071.001)

- **Action:** Configure an Intrusion Detection System (IDS) to alert when sensitive endpoints are accessed by `curl`, `wget`, or Python scripts instead of legitimate browsers.
