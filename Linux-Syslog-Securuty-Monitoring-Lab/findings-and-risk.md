# Incident Findings & Risk Analysis

## Executive Summary

During this lab, an attack simulation was conducted against the local Ubuntu environment. The investigation successfully detected the simulated brute-force attempts, privilege escalation attempts, and administrative activity. Due to the use of modern `systemd-journald` logging, traditional text-based `grep` on `/var/log/auth.log` was insufficient, requiring `journalctl` for accurate event correlation.

## Finding 1: Failed SSH Authentication Attempts

- **Observation:** Analysis revealed multiple failed password attempts for the invalid user `wronguser` originating from `localhost`.
- **Evidence:** `journalctl -u ssh | grep -i "failed"`
- **Metric:** 18 authentication failures detected.
- **Risk Level:** Low in this lab context (simulated locally). **High** in production.
- **Analysis:** This behavior indicates a brute-force attempt or misconfigured service. If this were a public server, the source IP would be blocked by a tool like Fail2Ban.

## Finding 2: Failed Privilege Escalation (`su`)

- **Observation:** An attempt was made to switch to the root account using `su -`. The attempt failed due to an incorrect password.
- **Evidence:** `su: Authentication failure`
- **Risk Level:** Medium.
- **Analysis:** Repeated `su` failures from a standard user can indicate an attacker attempting to escalate privileges.

## Finding 3: Administrative Activity Audit (`sudo`)

- **Observation:** A total of **158 sudo-related events** were logged. Recent events show `vboxuser` executing commands as root.
- **Evidence:** `sudo grep -ac "sudo" /var/log/auth.log` and `sudo tail -10 /var/log/auth.log`
- **Risk Level:** Informational.
- **Analysis:** Represents normal administrative behavior. Reviewing `sudo` commands is critical to detect if an insider threat is abusing privileges.

## Finding 4: System Log Volume & Noise

- **Observation:** The log contained 71 "session opened" and some "session closed" events.
- **Risk Level:** Informational.
- **Analysis:** Highlights "log noise." SOC analysts must filter by specific event IDs, usernames, or IPs to find the signal.

## MITRE ATT&CK Mapping

| Simulated Activity            | Tactic               | Technique                        | ID            |
| :---------------------------- | :------------------- | :------------------------------- | :------------ |
| Multiple failed SSH passwords | Credential Access    | Brute Force                      | **T1110**     |
| Failed root switch via `su -` | Privilege Escalation | Abuse Elevation Control: Sudo/Su | **T1548**     |
| Successful `sudo` execution   | Defense Evasion      | Valid Accounts: Local Accounts   | **T1078.003** |
