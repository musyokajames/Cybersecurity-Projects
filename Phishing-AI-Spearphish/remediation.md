# Remediation & Hardening

## 1. Enforce Strict DMARC

- **Action:** Change the DMARC policy for the company domain from `p=NONE` to `p=REJECT`. This tells receiving mail servers to outright reject any email claiming to be from `yourcompany.com` that fails authentication.

## 2. Email Gateway Rules

- **Action:** Configure the Secure Email Gateway to flag or block emails where the `X-Mailer` header is a scripting language (like PHP, Python, or Perl) when claiming to be from an internal executive.

## 3. Business Process Controls

- **Action:** Implement a mandatory verification policy for all financial requests sent via email. The recipient must call the requester on a known internal number to verify the task.
- **Action:** Educate executives and their assistants on the "Zero-Payload" BEC tactic.
