# Command Reference & Analysis: Living off the Land (LotL) Threat Hunt

This document outlines the commands used to simulate and hunt for Living off the Land (LotL) techniques. LotL attacks use native system tools to evade detection.

## Phase 1: System Baseline

_Establishing what "normal" looks like before the attack._

| Command            | Purpose                                                                                                                  |
| :----------------- | :----------------------------------------------------------------------------------------------------------------------- |
| `ls -la /tmp`      | Lists all files in the `/tmp` directory. This is a favorite hiding spot for attackers because everyone has write access. |
| `ss -tulpn`        | Lists all listening network ports to establish our baseline network footprint.                                           |
| `cat /etc/crontab` | Displays the system-wide cron schedule. Normally, this should just contain standard system comments.                     |

## Phase 2: Attack Simulation (LotL)

_Acting as the adversary using native tools to hide and persist._

| Command                                                                           | Purpose                                                                                                     |
| :-------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------- |
| `echo "malicious_payload" > /tmp/.hidden_backdoor.sh`                             | Creates a hidden file (due to the `.` prefix).                                                              |
| `chmod +x /tmp/.hidden_backdoor.sh`                                               | Makes the hidden file executable.                                                                           |
| `echo "* * * * * root /tmp/.hidden_backdoor.sh" \| sudo tee /etc/cron.d/backdoor` | **Persistence:** Drops a cron job into `/etc/cron.d/` to run the payload every minute as root.              |
| `python3 -m http.server 4444 &`                                                   | **Command & Control (C2):** Simulates a backdoor listening for a connection on port 4444 in the background. |

## Phase 3: Threat Hunting

_Hunting for the Indicators of Compromise (IOCs) we just created._

| Command                        | Purpose                                                                          |
| :----------------------------- | :------------------------------------------------------------------------------- |
| `cat /etc/cron.d/backdoor`     | Checking the specific cron directory to find the malicious job.                  |
| `find /tmp -name ".*" -type f` | Threat Hunting: Specifically searches `/tmp` for hidden files (`.*`).            |
| `ss -tulpn \| grep 4444`       | Filters network connections to find any process listening on our malicious port. |

## Phase 4: Incident Response & Verification

_Killing the active threat and proving the system is clean._

| Command                            | Purpose                                                                       |
| :--------------------------------- | :---------------------------------------------------------------------------- |
| `sudo lsof -i:4444`                | Finds the Process ID (PID) of the malicious listener running on port 4444.    |
| `sudo kill -9 <PID>`               | Forcefully terminates the malicious process to cut off the attacker's access. |
| `sudo rm /etc/cron.d/backdoor`     | Removes the malicious persistence mechanism.                                  |
| `sudo rm /tmp/.hidden_backdoor.sh` | Removes the hidden payload.                                                   |
| `ss -tulpn \| grep 4444`           | **Verification:** Re-checks the port to prove the backdoor is gone.           |
