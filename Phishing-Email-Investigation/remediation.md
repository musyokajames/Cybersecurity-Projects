# Remediation & Hardening

## 1. Stop the Attack

- **Action:** Block the IP address `198.51.100.42` at the firewall.
- **Action:** Alert the user who received the email to ensure they did not reply or purchase gift cards.

## 2. Email Security Gateway (SEG) Rules

- **Action:** Configure the SEG to flag emails where the `Reply-To` domain does not match the `From` domain.
- **Action:** Enable external email warning banners (e.g., "WARNING: This email originated from outside the organization") to prevent impersonation.

## 3. AI & Automation Policy

- **Action:** Ensure analysts understand that AI cannot replace manual context-checking. Configure detection rules that trigger on BEC keywords ("gift cards", "urgent", "wire transfer") regardless of the email's SPF/DKIM status.
