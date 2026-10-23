# Command Reference & Analysis

This document outlines the commands used in the lab, organized by the investigation phases, including the analytical reasoning for why each command was executed.

## Phase 1: System Baseline & Preparation

_Establishing the environment context and ensuring logging is active._

| Command                    | Purpose                                                                                                  |
| :------------------------- | :------------------------------------------------------------------------------------------------------- |
| `hostname`                 | Identifies the name of the machine to anchor the investigation.                                          |
| `whoami`                   | Confirms the current user context (low-privilege `vboxuser`).                                            |
| `lsb_release -a`           | Identifies the OS version. Crucial for knowing which log formats to expect.                              |
| `uname -r`                 | Displays the kernel version for environment documentation.                                               |
| `uptime`                   | Shows how long the system has been running and current load.                                             |
| `systemctl status rsyslog` | **Critical:** Verifies that the system logging daemon is active. If this is dead, no logs are generated. |

## Phase 2: Logging Infrastructure Analysis

_Locating where evidence is stored and how logs are routed._

| Command                                  | Purpose                                                                                          |
| :--------------------------------------- | :----------------------------------------------------------------------------------------------- |
| `ls -lh /var/log/`                       | Lists log files with human-readable file sizes to identify the most active logs.                 |
| `grep "module" /etc/rsyslog.conf`        | Inspects the rsyslog configuration to see which modules (`imuxsock`, `imklog`) handle ingestion. |
| `grep "IncludeConfig" /etc/rsyslog.conf` | Shows where additional log routing rules are stored (`/etc/rsyslog.d/*.conf`).                   |

## Phase 3: Attack Simulation

_Acting as the adversary to generate evidence._

| Command                   | Purpose                                                                                   |
| :------------------------ | :---------------------------------------------------------------------------------------- |
| `ssh wronguser@localhost` | Simulated a brute-force attack locally to generate "Failed password" events.              |
| `su -`                    | Simulated a privilege escalation attempt to the root account using an incorrect password. |

## Phase 4: Log Investigation & Event Correlation

_Hunting for the evidence generated in Phase 3._

| Command                                         | Purpose                                                                                                                                    |
| :---------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------- |
| `sudo grep "Failed password" /var/log/auth.log` | **Failed Attempt:** Attempted to find logins in the traditional text log, returning a "binary file matches" error.                         |
| `sudo journalctl -u ssh \| grep -i "failed"`    | **Pivot & Success:** Bypassed the binary log issue by querying `systemd-journald` for the SSH unit, successfully extracting failed logins. |

## Phase 5: Metrics & Privileged Activity Audit

_Quantifying events and reviewing sudo activity._

| Command                                                    | Purpose                                                                             |
| :--------------------------------------------------------- | :---------------------------------------------------------------------------------- |
| `sudo grep -ac "session opened" /var/log/auth.log`         | Uses `-a` (treat binary as text) and `-c` (count) to quantify total login sessions. |
| `sudo grep -ac "authentication failure" /var/log/auth.log` | Counts total authentication failures.                                               |
| `sudo grep -ac "sudo" /var/log/auth.log`                   | Counts total sudo events to establish an administrative baseline.                   |
| `sudo tail -10 /var/log/auth.log`                          | Displays the 10 most recent log entries, showing exact commands run by `vboxuser`.  |
