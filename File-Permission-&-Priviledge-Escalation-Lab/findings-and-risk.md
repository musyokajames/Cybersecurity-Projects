# Security Audit Findings & Risk Analysis

## Executive Summary

A security audit was performed on the Ubuntu environment to identify privilege escalation vectors and weak file permissions. The audit included baseline hunting, a simulation of an administrative misconfiguration (World-Writable files), and a review of SUID binaries and Linux Capabilities.

The audit successfully identified that while the default system was secure, a single administrative error could introduce a critical vulnerability.

---

## Finding 1: World-Writable System File (Simulated)

- **Observation:** During the baseline hunt, the system returned clean. However, after injecting a simulated misconfiguration (`chmod 777 /etc/vulnerable_config.conf`), the subsequent hunt successfully flagged the file.
- **Evidence:** `find / -type f -user root -perm -o+w 2>/dev/null`
- **Risk Level:** **Critical** (in a production environment).
- **Analysis:** A world-writable file owned by root means any low-privilege user on the system can modify it. If this file is a configuration file executed by a system service, an attacker can inject malicious code and execute it as root, resulting in full system compromise.

## Finding 2: SUID Binaries Review

- **Observation:** The system contained several legitimate SUID binaries (e.g., `/usr/bin/passwd`, `/usr/bin/sudo`).
- **Evidence:** `find / -perm -4000 -type f 2>/dev/null | head -n 15`
- **Risk Level:** Informational.
- **Analysis:** SUID binaries run with the privileges of the file owner. An attacker will actively search for this exact list to find "out-of-place" custom SUID binaries. Regular audits of this list are critical to detect backdoors.

---

## MITRE ATT&CK Mapping

| Audited Behavior / Attack Vector | Tactic               | Technique                              | ID            |
| :------------------------------- | :------------------- | :------------------------------------- | :------------ |
| Abusing World-Writable files     | Privilege Escalation | Hijack Execution Flow                  | **T1574**     |
| Searching for SUID binaries      | Discovery            | File and Directory Discovery           | **T1083**     |
| Abusing SUID for escalation      | Privilege Escalation | Abuse Elevation Control: Setuid/Setgid | **T1548.001** |

---

## Lessons Learned

- **Trust, but verify:** In a real audit, you cannot assume a fix worked. After performing remediation, the `find` command was run again to mathematically prove the vulnerability was gone.
- **The Danger of `chmod 777`:** It is a common shortcut for beginners to fix "Permission Denied" errors, but it creates a massive backdoor.
