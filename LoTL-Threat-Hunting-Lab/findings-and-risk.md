# Threat Hunt Findings & Risk Analysis

## Executive Summary

A simulated Living off the Land (LotL) attack was executed and investigated on an Ubuntu environment. The attack utilized native system binaries (`bash`, `cron`, `python3`) to establish persistence and open a command-and-control channel. The threat hunt successfully identified all simulated IOCs and performed successful incident response to neutralize the threat.

---

## Finding 1: Hidden Payload in `/tmp`

- **Observation:** A malicious script was created with a `.` prefix (`/tmp/.hidden_backdoor.sh`), making it invisible to standard `ls` commands.
- **Evidence:** `find /tmp -name ".*" -type f`
- **Risk Level:** High.
- **Analysis:** Attackers use hidden files in world-writable directories like `/tmp` to stage payloads. Because `/tmp` is often cleared on reboot, attackers must combine this with a persistence mechanism (like cron) to survive.

## Finding 2: Unauthorized Persistence via Cron

- **Observation:** A malicious cron job was dropped into `/etc/cron.d/backdoor`.
- **Evidence:** `cat /etc/cron.d/backdoor`
- **Risk Level:** Critical.
- **Analysis:** By placing a file in `/etc/cron.d/`, the attacker ensured their payload ran every minute as the root user. This is a classic persistence technique (T1053.003).

## Finding 3: Active Backdoor on Port 4444

- **Observation:** A `python3` process was listening on port 4444, simulating a reverse shell.
- **Evidence:** `ss -tulpn | grep 4444` and `sudo lsof -i:4444`
- **Risk Level:** Critical.
- **Analysis:** Port 4444 is a default port for Metasploit and is highly suspicious. The attacker used Python (a trusted binary) to open the port, evading basic antivirus.

---

## MITRE ATT&CK Mapping

| Simulated Activity                | Tactic              | Technique                    | ID            |
| :-------------------------------- | :------------------ | :--------------------------- | :------------ |
| Hiding payloads using `.` prefix  | Defense Evasion     | Hidden Files and Directories | **T1564.001** |
| Creating cron job for persistence | Persistence         | Scheduled Task/Job: Cron     | **T1053.003** |
| Using Python for C2               | Command and Control | Application Layer Protocol   | **T1071**     |

---

## Lessons Learned

- **The `find` command is essential:** Standard directory listings miss hidden files. `find / -name ".*"` is a mandatory check during incident response.
- **Native binaries evade detection:** The attacker didn't download a hacking tool; they used `python3`, which is already installed. Monitoring _how_ built-in tools are used (like Python opening a network port) is critical.
