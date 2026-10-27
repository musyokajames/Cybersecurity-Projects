# Remediation & Hardening

## 1. Block the Indicators

- **Action:** Block the domain `docusign-tdbank.com.ph` at the DNS or Web Gateway level.
- **Action:** Submit the URL to threat intelligence platforms (like VirusTotal) to protect other organizations.

## 2. Security Awareness Training

- **Action:** Train users to inspect URLs before clicking. A user should notice that `.com.ph` is not the official DocuSign domain.
- **Action:** Teach users to navigate directly to `docusign.com` rather than clicking links in emails.

## 3. Email Gateway Configuration

- **Action:** Configure the Secure Email Gateway (SEG) to detonate links in a sandbox before delivering the email to the user's inbox (Time-of-Click protection).
