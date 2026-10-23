# Command Reference & Analysis: Linux Privilege Audit

This document outlines the commands used to audit a Linux system, inject a simulated misconfiguration, and hunt for privilege escalation vectors.

## Phase 1: System Baseline & Identity

_Establishing the current user context and understanding what privileges we already have._

| Command         | Purpose                                                             |
| :-------------- | :------------------------------------------------------------------ |
| `id`            | Displays the current user's UID, GID, and group memberships.        |
| `sudo -l`       | Lists the commands the current user is allowed to run with `sudo`.  |
| `ls -la /home/` | Lists home directories to check if other users' files are readable. |

## Phase 2: Baseline Threat Hunting (Clean System)

_Searching for vulnerabilities before they are created to establish a baseline._

| Command                                            | Purpose                                                                                                                           |
| :------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| `find / -type f -user root -perm -o+w 2>/dev/null` | **Critical World-Writable:** Searches for files owned by root that _anyone_ can modify. On a secure system, this returns nothing. |

## Phase 3: Vulnerability Injection

_Simulating a common administrator error._

| Command                                      | Purpose                                                                |
| :------------------------------------------- | :--------------------------------------------------------------------- |
| `sudo touch /etc/vulnerable_config.conf`     | Creates a simulated system configuration file.                         |
| `sudo chmod 777 /etc/vulnerable_config.conf` | **The Flaw:** Grants full read/write/execute permissions to all users. |

## Phase 4: Threat Hunting - Finding the Flaw

_Running the audit and filtering the output to highlight the vulnerability._

| Command                                                                           | Purpose                                                                                                               |
| :-------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------- |
| `find / -type f -user root -perm -o+w 2>/dev/null`                                | Re-running the scan to detect the file among system noise.                                                            |
| `find / -type f -user root -perm -o+w 2>/dev/null \| grep -E "/etc\|/tmp\|/home"` | **Filtered Hunt:** Pipes the output to `grep` to only show files in high-risk directories, cutting through the noise. |
| `ls -la /etc/vulnerable_config.conf`                                              | Verifying the permissions (`rwxrwxrwx`).                                                                              |

## Phase 5: SUID & Capabilities Audit

_Searching for binaries that run with elevated privileges._

| Command                                                | Purpose                                                                               |
| :----------------------------------------------------- | :------------------------------------------------------------------------------------ |
| `find / -perm -4000 -type f 2>/dev/null \| head -n 15` | **SUID Discovery:** Finds SUID files, using `head` to only show the first 15 results. |
| `getcap -r / 2>/dev/null`                              | **Capabilities:** Searches for binaries with special Linux capabilities.              |

## Phase 6: Remediation & Verification

_Fixing the vulnerability and proving the fix was successful._

| Command                                            | Purpose                                                                      |
| :------------------------------------------------- | :--------------------------------------------------------------------------- |
| `sudo chmod 644 /etc/vulnerable_config.conf`       | Restores normal, secure permissions.                                         |
| `sudo rm /etc/vulnerable_config.conf`              | Deletes the simulated vulnerable file.                                       |
| `find / -type f -user root -perm -o+w 2>/dev/null` | **Verification:** Re-runs the scan to prove the world-writable file is gone. |
