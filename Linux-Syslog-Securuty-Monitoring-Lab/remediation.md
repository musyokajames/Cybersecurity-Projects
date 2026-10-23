# Remediation & Hardening Recommendations

Based on the simulated attack and the findings from the log analysis, the following remediation steps are recommended to harden the Ubuntu environment.

## 1. Mitigate Brute-Force Attacks (T1110)

**Finding:** Multiple failed SSH login attempts were detected.

- **Action 1: Implement Fail2Ban.** Install and configure `fail2ban` to automatically monitor logs and block IP addresses that exceed a threshold of failed login attempts.
- **Action 2: Disable Password Authentication for SSH.** Enforce SSH key-based authentication. Edit `/etc/ssh/sshd_config` and set `PasswordAuthentication no` and `ChallengeResponseAuthentication no`.

## 2. Restrict Privileged Access (T1548 & T1078)

**Finding:** Failed `su` attempts and heavy `sudo` usage were observed.

- **Action 1: Restrict `su` access.** Ensure that only members of the `sudo` group can use the `su` command.
- **Action 2: Principle of Least Privilege.** Review `/etc/sudoers` (using `visudo`). Ensure users only have the exact permissions required.
- **Action 3: Disable Root SSH Login.** Ensure `PermitRootLogin no` is set in `/etc/ssh/sshd_config`.

## 3. Improve Logging & Monitoring

**Finding:** Traditional `/var/log/auth.log` text files were unreliable due to binary `systemd-journald` logs.

- **Action 1: Centralized Logging (SIEM).** Configure `rsyslog` or `journald` to forward logs to a centralized SIEM (like Splunk or Wazuh). If an attacker compromises the host, they cannot delete remote logs.
- **Action 2: Enable Auditd.** Configure the Linux Audit daemon (`auditd`) to monitor file access and system calls.
- **Action 3: Log Tampering Prevention.** Ensure logs are appended (via `chattr`) or stored remotely to prevent attackers from clearing tracks.
