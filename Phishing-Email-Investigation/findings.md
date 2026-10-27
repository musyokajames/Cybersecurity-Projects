# Investigation Findings: BEC Gift Card Scam

## Finding 1: DMARC Authentication Failure

- **Observation:** PhishTool reported `DMARC=Fail` for the email.
- **Risk Level:** High.
- **Analysis:** The attacker spoofed the `From` address (`ceo@company.com`), but the sending IP was not authorized in the domain's DMARC record. This is the primary technical indicator of a spoofing attempt.

## Finding 2: Reply-To Mismatch

- **Observation:** The `Reply-To` address is `ceo.private@gmail.com`.
- **Risk Level:** Critical.
- **Analysis:** This is a definitive BEC indicator. The attacker wants the victim to reply to an external, attacker-controlled inbox, bypassing the company's internal email security.

## Finding 3: Display Name Spoofing

- **Observation:** The display name is "CEO John Doe", but the actual `From` email address is a generic support/bounce address.
- **Risk Level:** Medium.
- **Analysis:** Attackers use display name spoofing because mobile email clients often only show the display name, tricking the user into believing it is a legitimate internal email.

---

## MITRE ATT&CK Mapping

| Tactic         | Technique                           | ID        |
| :------------- | :---------------------------------- | :-------- |
| Initial Access | Phishing: Spearphishing via Service | T1566.003 |
| Reconnaissance | Search Open Websites/Domains        | T1593     |
