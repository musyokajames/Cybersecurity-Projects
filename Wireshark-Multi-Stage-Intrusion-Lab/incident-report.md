# Incident Report: Multi-Stage Network Intrusion

## Incident Metadata

- **Incident ID:** INC-2026-1025-NET
- **Severity:** High
- **Status:** Closed (Simulated)
- **Analyst:** SOC Analyst (Portfolio Lab)
- **Evidence:** `attack.pcap`

## Executive Summary

A simulated network intrusion was detected and analyzed via packet capture. The attacker performed network reconnaissance, delivered a simulated malware payload, and established a Command & Control (C2) channel. During the C2 session, simulated plaintext credentials were exfiltrated.

## Timeline of Events (Reconstructed from PCAP)

| Phase            | Action                                                                         | MITRE ATT&CK                                   |
| :--------------- | :----------------------------------------------------------------------------- | :--------------------------------------------- |
| **Recon**        | Attacker scans ports 21, 22, 80, 443, 4444, 8080.                              | T1046 (Network Service Discovery)              |
| **Infection**    | Victim host requests `GET /malware.exe` from the C2 server on port 4444.       | T1071 (Application Layer Protocol)             |
| **C2**           | Malware establishes 5 distinct connections to port 4444 at 1-second intervals. | T1102 (Web Service)                            |
| **Exfiltration** | Malware transmits `USER=admin&PASS=SuperSecret123` to the C2 server.           | T1048 (Exfiltration Over Alternative Protocol) |

## Root Cause Analysis

- **Cause:** The victim host was allowed to make outbound connections to an unapproved port (4444) and download unverified executables.
- **Impact:** Simulated full compromise of user credentials.

## Indicators of Compromise (IOCs)

- **IP Address:** 127.0.0.1 (Simulated C2 Server)
- **Port:** 4444 (Common Metasploit default)
- **URI:** `/malware.exe`
- **Payload Signature:** `USER=admin&PASS=SuperSecret123`

## Recommendations

1. **Egress Filtering:** Block outbound traffic on non-standard ports (like 4444).
2. **IDS/IPS Signatures:** Alert on rapid connection attempts (port scans) followed by beaconing behavior.
3. **Encryption:** Enforce TLS for all internal and external communications to prevent credential sniffing.
