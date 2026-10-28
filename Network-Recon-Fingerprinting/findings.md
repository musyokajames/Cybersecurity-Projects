# Assessment Findings: Network Reconnaissance

## Finding 1: Exposed SSH Service

- **Observation:** Port 22 is open and accessible on the target VM.
- **Risk Level:** Medium.
- **Analysis:** Exposing SSH to a network is necessary for administration but increases the attack surface. If weak passwords or vulnerable SSH versions are used, this can lead to remote code execution or unauthorized access.

## Finding 2: Service Version Disclosure

- **Observation:** The SSH server broadcasts its exact version (`OpenSSH 10.2p1 Ubuntu`).
- **Risk Level:** Low.
- **Analysis:** Banner grabbing makes the attacker's job easy. While it is difficult to disable this completely, administrators should ensure the service is patched immediately when a CVE is released for that specific version.

## Finding 3: Lack of Intrusion Prevention (Defender Analysis)

- **Observation:** The Nmap scan was able to run uninterrupted, and Wireshark captured the full SYN flood.
- **Risk Level:** High.
- **Analysis:** The target machine lacks a tool like `fail2ban` or an IDS (like Snort) to detect and block the rapid connection attempts indicative of a port scan.

---

## MITRE ATT&CK Mapping

| Tactic    | Technique                    | ID    |
| :-------- | :--------------------------- | :---- |
| Discovery | Network Service Discovery    | T1046 |
| Discovery | System Information Discovery | T1082 |
