# Traffic Analysis Findings

## Finding 1: Nmap Port Scan

- **Observation:** A rapid succession of TCP `[SYN]` packets was sent to various ports on the localhost without completing the TCP handshake.
- **Evidence:** `tcp.flags.syn == 1 && tcp.flags.ack == 0`
- **Risk Level:** Medium.
- **Analysis:** This indicates network reconnaissance. An attacker (or malware) is probing for open services to exploit.

## Finding 2: Plaintext Credential Transmission

- **Observation:** A TCP packet on port 8080 contained cleartext data reading `username=admin password=SuperSecret123`.
- **Evidence:** `tcp.port == 8080` -> Inspect Packet Bytes.
- **Risk Level:** Critical.
- **Analysis:** Sending credentials over unencrypted protocols (like HTTP or Telnet) allows any attacker performing a Man-in-the-Middle (MitM) attack to easily steal passwords.

---

## MITRE ATT&CK Mapping

| Simulated Activity    | Tactic            | Technique                 | ID        |
| :-------------------- | :---------------- | :------------------------ | :-------- |
| Nmap Port Scan        | Discovery         | Network Service Discovery | **T1046** |
| Plaintext Credentials | Credential Access | Network Sniffing          | **T1040** |
