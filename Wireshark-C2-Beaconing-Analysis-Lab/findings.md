# Traffic Analysis Findings

## Finding 1: Automated C2 Beaconing

- **Observation:** A workstation is repeatedly connecting to port 4444 at an exact 2.000-second interval.
- **Evidence:** `tcp.port == 4444` and Frame Time Delta.
- **Risk Level:** Critical.
- **Analysis:** This rhythmic pattern is the hallmark of a C2 beacon. The malware is checking in with an attacker to see if it should execute a new command, download a payload, or begin exfiltration.

## Finding 2: Suspicious Port Usage

- **Observation:** The traffic is communicating over port 4444.
- **Evidence:** Packet Destination Port.
- **Risk Level:** High.
- **Analysis:** While port 4444 can be used legitimately, it is overwhelmingly associated with hacking tools, specifically the Metasploit framework. In a corporate network, this port should be blocked at the firewall unless there is a documented business exception.

---

## MITRE ATT&CK Mapping

| Simulated Activity       | Tactic              | Technique                  | ID    |
| :----------------------- | :------------------ | :------------------------- | :---- |
| Phoning home to attacker | Command and Control | Application Layer Protocol | T1071 |
| Regular timed intervals  | Command and Control | Web Service                | T1102 |
