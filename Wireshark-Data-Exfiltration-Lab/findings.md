# Traffic Analysis Findings

## Finding 1: Anomalous Outbound Traffic

- **Observation:** Wireshark Endpoint statistics revealed a single connection transferring ~42 MB of data in a few seconds.
- **Evidence:** `Statistics > Endpoints`
- **Risk Level:** High.
- **Analysis:** While 44 MB is small for a large file download, it is highly abnormal for raw text transmission. This behavior indicates an automated script exfiltrating data rather than normal user browsing.

## Finding 2: Simulated PII Exfiltration

- **Observation:** Following the TCP stream revealed thousands of lines of structured data containing fake Social Security Numbers and usernames.
- **Evidence:** `Follow > TCP Stream`
- **Risk Level:** Critical.
- **Analysis:** The attacker is transmitting sensitive data in plain text. If this were a real company, it would be a reportable data breach.

---

## MITRE ATT&CK Mapping

| Simulated Activity   | Tactic       | Technique                                     | ID            |
| :------------------- | :----------- | :-------------------------------------------- | :------------ |
| Sending data out     | Exfiltration | Exfiltration Over C2 Channel                  | **T1041**     |
| Unencrypted Transfer | Exfiltration | Exfiltration Over Unencrypted Non-C2 Protocol | **T1048.003** |
