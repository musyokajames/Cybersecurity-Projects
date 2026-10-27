# Investigation Findings: Malicious Macro

## Finding 1: AutoOpen Execution Trigger

- **Observation:** The VBA macro utilizes the `Sub AutoOpen()` function.
- **Risk Level:** Critical.
- **Analysis:** This ensures the malicious code executes automatically when the document is opened, requiring no further interaction from the user other than clicking "Enable Content."

## Finding 2: Obfuscated PowerShell Execution

- **Observation:** The macro invokes `powershell.exe` with the `-NoP` (No Profile), `-W Hidden` (Hidden Window), and `-Exec Bypass` (Bypass Execution Policy) flags.
- **Risk Level:** Critical.
- **Analysis:** These flags are specifically designed to evade detection and hide the malicious activity from the user. The `-Enc` flag indicates the payload is Base64 encoded.

## Finding 3: Malicious Downloader

- **Observation:** After decoding the Base64 string, the script reveals a call to download a file from `http://evil.com/payload.exe` and execute it.
- **Risk Level:** Critical.
- **Analysis:** This is a "dropper" technique. The macro is just the delivery mechanism; the real malware is hosted remotely and will be executed in memory or saved to disk.

---

## MITRE ATT&CK Mapping

| Tactic          | Technique                                         | ID        |
| :-------------- | :------------------------------------------------ | :-------- |
| Initial Access  | Phishing: Spearphishing Attachment                | T1566.001 |
| Execution       | Command and Scripting Interpreter: Visual Basic   | T1059.005 |
| Execution       | Command and Scripting Interpreter: PowerShell     | T1059.001 |
| Defense Evasion | Obfuscated Files or Information: Software Packing | T1027.002 |
