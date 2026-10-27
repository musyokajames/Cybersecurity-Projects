# Remediation & Hardening

## 1. Endpoint Protection

- **Action:** Configure Microsoft Office (or LibreOffice) via Group Policy to disable macros by default. Only allow macros signed by trusted publishers.
- **Action:** Deploy Endpoint Detection and Response (EDR) to monitor for `powershell.exe` being spawned by `winword.exe`.

## 2. Email Security Gateway (SEG)

- **Action:** Configure the SEG to strip or quarantine `.docm`, `.xlsm`, and `.js` attachments from external senders.
- **Action:** Add `evil.com` to the blocklist (though attackers rarely reuse the same domain).

## 3. User Awareness

- **Action:** Train users to never click "Enable Content" on unexpected documents, even if they appear to come from a known contact.
