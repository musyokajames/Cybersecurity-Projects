# Incident Report: INC-2026-1024

## Incident Metadata

- **Incident ID:** INC-2026-1024
- **Report Date:** October 24, 2026
- **Analyst:** vboxuser (SOC Analyst Lab Simulation)
- **Affected Asset:** `LogLabEnv` (Ubuntu LTS / VirtualBox)
- **Severity:** **High** (Simulated Environment)
- **Status:** Closed

---

## Executive Summary

A simulated Living off the Land (LotL) attack was conducted to test the environment's threat-hunting capabilities. The adversary utilized native system binaries to hide a payload, establish persistence via cron, and open a command-and-control backdoor on port 4444. Through manual threat hunting, the analyst successfully identified the IOCs, killed the malicious process, and removed the persistence mechanisms.

---

## Technical Root Cause Analysis

### 1. Hidden File Creation

- **Cause:** The `/tmp` directory allows any user to write and execute files.
- **Impact:** The attacker staged a hidden payload (`/tmp/.hidden_backdoor.sh`), evading standard directory listing commands.

### 2. Persistence via Cron

- **Cause:** The `/etc/cron.d/` directory was not being monitored for file integrity changes.
- **Impact:** The attacker ensured their payload would execute every minute as the root user, establishing a permanent foothold.

### 3. Command & Control (C2)

- **Cause:** The system allowed outbound listeners from standard scripting languages.
- **Impact:** An active backdoor was opened on port 4444.

---

## Timeline of Events (UTC)

| Time      | Event Description                                                           |
| :-------- | :-------------------------------------------------------------------------- |
| **14:00** | Baseline established. No hidden files in `/tmp` and no unusual ports open.  |
| **14:10** | **Attack:** Hidden payload created and cron job injected.                   |
| **14:15** | **Attack:** C2 backdoor opened on port 4444.                                |
| **14:20** | **Detection:** Analyst detects modified cron directory.                     |
| **14:25** | **Detection:** Analyst identifies hidden file in `/tmp`.                    |
| **14:30** | **Detection:** Analyst identifies active listener on port 4444.             |
| **14:35** | **Containment:** Analyst identifies PID and forcefully kills the process.   |
| **14:40** | **Eradication:** Analyst removes malicious cron file and hidden payload.    |
| **14:45** | **Verification:** Analyst verifies port 4444 is closed and system is clean. |

---

**Report Status:** Finalized and filed.
