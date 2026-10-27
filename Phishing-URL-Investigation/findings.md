# Investigation Findings: DocuSign/TD Bank Phishing

## Finding 1: Typosquatting Domain

- **Observation:** The URL uses `docusign-tdbank.com.ph`. The top-level domain (TLD) is `.ph` (Philippines), not `.com`.
- **Risk Level:** Critical.
- **Analysis:** The attacker combined two trusted brands (DocuSign and TD Bank) to create a sense of urgency and legitimacy.

## Finding 2: Credential Harvesting Page

- **Observation:** The URLScan.io screenshot shows a functional login page requesting user credentials.
- **Risk Level:** Critical.
- **Analysis:** This is a classic credential harvesting attack. Any credentials entered are sent directly to the attacker.

## Finding 3: Hosting Infrastructure

- **Observation:** The domain resolves to `45.79.222.138`, hosted on Akamai/Linode.
- **Risk Level:** High.
- **Analysis:** Attackers frequently use legitimate cloud hosting providers (like Linode) because blocking the entire IP range would cause massive collateral damage to legitimate businesses.

---

## MITRE ATT&CK Mapping

| Tactic               | Technique                       | ID        |
| :------------------- | :------------------------------ | :-------- |
| Initial Access       | Phishing: Spearphishing Link    | T1566.002 |
| Resource Development | Acquire Infrastructure: Domains | T1583.001 |
