# Traffic Analysis Findings

## Finding 1: Cleartext Web Traffic (HTTP)

- **Observation:** The user visited `neverssl.com` over port 80 (HTTP) rather than port 443 (HTTPS).
- **Evidence:** `http` filter -> `GET / HTTP/1.1` packet.
- **Risk Level:** Medium.
- **Analysis:** HTTP traffic is unencrypted. Any data transmitted (passwords, session tokens, or sensitive data) can be intercepted by an attacker on the same network.

## Finding 2: Unusual User-Agent

- **Observation:** The HTTP request contained a `User-Agent` header identifying the client as `curl/7.xx.x` rather than a standard web browser (like Firefox or Chrome).
- **Evidence:** Expand Hypertext Transfer Protocol in the `GET` packet.
- **Risk Level:** Informational.
- **Analysis:** In a corporate environment, seeing `curl` or `wget` in web logs often indicates automated scripts, system administration tasks, or a malicious actor downloading a payload via the command line rather than a browser.

---

## MITRE ATT&CK Mapping

| Simulated Activity     | Tactic              | Technique                                 | ID            |
| :--------------------- | :------------------ | :---------------------------------------- | :------------ |
| Web Request via Script | Command and Control | Application Layer Protocol: Web Protocols | **T1071.001** |
| Unencrypted Web Data   | Credential Access   | Network Sniffing                          | **T1040**     |
