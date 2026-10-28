# Remediation & Hardening

## 1. Business Process Controls

- **Action:** Implement a strict dual-authorization policy for any changes to vendor banking details. This must require a voice call to a pre-verified contact number (not one provided in the suspicious email).
- **Action:** Train finance teams to recognize the signs of BEC and thread hijacking.

## 2. Vendor Security Recommendations

- **Action:** Notify the vendor immediately that their account is compromised.
- **Action:** Recommend that the vendor enforce Multi-Factor Authentication (MFA) on all email accounts, preferably using phishing-resistant FIDO2 tokens (like YubiKeys) rather than SMS.

## 3. Email Gateway Monitoring

- **Action:** Configure the Secure Email Gateway to alert on "Impossible Travel" events for frequent external contacts. If an established vendor suddenly emails from a new geographic region, flag it for review.
