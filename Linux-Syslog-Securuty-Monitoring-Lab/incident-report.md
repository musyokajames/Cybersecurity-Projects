# Incident Report: INC-2026-1022

## Incident Metadata

- **Incident ID:** INC-2026-1022
- **Report Date:** October 22, 2026
- **Analyst:** vboxuser (SOC Analyst Lab Simulation)
- **Affected Asset:** `LogLabEnv` (Ubuntu LTS / VirtualBox)
- **Severity:** **Low** (Simulated Environment)
- **Status:** Closed

---

## Executive Summary

On October 22, 2026, a simulated security incident was executed and investigated on the `LogLabEnv` virtual machine. The simulation involved a local SSH brute-force attempt and a failed privilege escalation attempt (`su -`).

Through log analysis using `journalctl`, the analyst successfully detected the failed authentication attempts and audited subsequent administrative (`sudo`) activity. No actual system compromise occurred. The exercise highlighted the need for stricter SSH configurations and centralized logging.

---

## Timeline of Events (UTC)

| Time      | Event Description                                                                                             |
| :-------- | :------------------------------------------------------------------------------------------------------------ |
| **00:46** | System baseline established. `rsyslog` verified as active.                                                    |
| **01:11** | **Attack Begins:** First failed SSH login for user `wronguser` from `127.0.0.1`.                              |
| **01:13** | Multiple subsequent failed SSH login attempts detected.                                                       |
| **01:17** | Analyst initiated administrative activity via `sudo`.                                                         |
| **01:19** | **Escalation Attempt:** Analyst attempts `su -` with an invalid password. Logged as `Authentication failure`. |
| **01:21** | **Detection:** Analyst begins hunting logs, discovering a "binary file matches" error in `auth.log`.          |
| **01:23** | **Pivot:** Analyst uses `journalctl` to query binary logs and isolates failed SSH attempts.                   |
| **01:25** | Analysis completed. Event counts and MITRE ATT&CK mapping documented.                                         |

---

## Technical Root Cause Analysis

### 1. Brute Force Attempt (T1110)

- **Cause:** The SSH service was configured to allow password-based authentication.
- **Impact:** An attacker could repeatedly guess passwords.
- **Detection:** `journalctl -u ssh | grep -i "failed"` successfully identified the failed password attempts.

### 2. Failed Privilege Escalation (T1548)

- **Cause:** The standard user attempted to switch to root using `su -` with an incorrect password.
- **Impact:** If successful, the attacker would have gained full administrative control.
- **Detection:** `su: Authentication failure` logged in the authentication journal.

---

## Resolution & Next Steps

The incident was contained and resolved within the lab environment. To prevent similar incidents in production, the following steps (detailed in `remediation.md`) must be implemented:

1. **Disable Password Authentication:** Enforce SSH keys.
2. **Implement Fail2Ban:** Automatically ban IPs generating repeated failures.
3. **Deploy Centralized Logging:** Forward logs to a SIEM to prevent tampering.
4. **Restrict `su` Access:** Enforce `sudo` usage with strict `sudoers` rules.
