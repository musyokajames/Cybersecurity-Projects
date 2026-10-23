# Remediation & Hardening Recommendations

Based on the security audit, the following remediation steps and hardening recommendations are advised for Linux environments to prevent privilege escalation.

## 1. Mitigate World-Writable Files (T1574)

**Finding:** A simulated world-writable file (`/etc/vulnerable_config.conf`) owned by root was created and detected.

- **Action 1: Continuous Auditing.** Run automated scripts weekly to search for world-writable files owned by root (`find / -type f -user root -perm -o+w`). Any findings should be investigated immediately.
- **Action 2: Enforce Least Privilege.** Never use `chmod 777`. System configuration files should generally be `644` (read/write for root, read-only for others) or `600` (read/write only for root).
- **Action 3: File Integrity Monitoring (FIM).** Use tools like `AIDE` or `Tripwire` to alert administrators immediately if critical files are modified.

## 2. Secure SUID Binaries (T1548.001)

**Finding:** Standard system SUID binaries were identified.

- **Action 1: Baseline SUID Files.** Create a known-good baseline of all SUID binaries on the system. Routinely compare the live system against this baseline to detect newly added SUID binaries.
- **Action 2: Mount `/tmp` with `nosuid`.** Attackers often drop SUID payloads into `/tmp` or `/home`. Ensure these directories are mounted with the `nosuid` flag in `/etc/fstab`.
