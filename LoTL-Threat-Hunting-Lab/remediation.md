# Remediation & Hardening Recommendations

Based on the Living off the Land (LotL) threat hunt, the following remediation steps are recommended to prevent adversaries from hiding and persisting on Linux systems.

## 1. Detect and Prevent Hidden Files (T1564.001)

**Finding:** A hidden payload was found in `/tmp`.

- **Action 1: Monitor `/tmp`:** Use tools like `auditd` or a File Integrity Monitoring (FIM) tool to alert anytime a new executable file is created in `/tmp`.
- **Action 2: Mount `/tmp` Securely:** Mount the `/tmp` directory with the `noexec` flag in `/etc/fstab`. This prevents attackers from executing scripts directly from the temp directory.

## 2. Secure Cron and Persistence Mechanisms (T1053.003)

**Finding:** A malicious job was added to `/etc/cron.d/`.

- **Action 1: Restrict Permissions:** Ensure `/etc/cron.d/`, `/etc/crontab`, and `/var/spool/cron/` are strictly owned by root and not writable by standard users.
- **Action 2: File Integrity Monitoring:** Use `AIDE` or `Tripwire` to monitor the cron directories for unauthorized changes and alert the SOC immediately.

## 3. Detect Unauthorized Network Listeners

**Finding:** Python was used to open port 4444.

- **Action 1: Network Monitoring:** EDR tools should be configured to alert whenever a scripting language (like Python, Perl, or Ruby) attempts to open a listening port.
- **Action 2: Host-Based Firewall:** Implement `ufw` or `iptables` to deny all incoming traffic by default, only allowing explicitly required ports (like 22 for SSH).
