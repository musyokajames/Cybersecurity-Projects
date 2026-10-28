# Investigation Findings

## Finding 1: Account Compromise (Impossible Travel)

- **Observation:** The email originated from `102.89.33.1`, which geolocates to Lagos, Nigeria.
- **Risk Level:** Critical.
- **Analysis:** The vendor's legitimate operations are based in the US. This anomaly indicates that the vendor's email credentials were likely stolen via a prior phishing attack or credential stuffing, and the attacker logged in from a foreign location to send the fraudulent invoice.

## Finding 2: Financial Redirection

- **Observation:** The email instructed the victim to change banking details to a Deutsche Bank account in Frankfurt.
- **Risk Level:** Critical.
- **Analysis:** This is the payload of the thread hijacking attack. The attacker is attempting to divert the invoice payment to a mule account they control.

## Finding 3: Urgency and Context

- **Observation:** The subject included "URGENT" and the email replied directly to an existing thread (`In-Reply-To` header present).
- **Risk Level:** Medium.
- **Analysis:** By replying to an existing thread, the attacker inherits the trust of the previous conversation, lowering the victim's guard.

---

## MITRE ATT&CK Mapping

| Tactic         | Technique                                 | ID        |
| :------------- | :---------------------------------------- | :-------- |
| Initial Access | Phishing: Spearphishing via Service       | T1566.003 |
| Collection     | Email Collection: Remote Email Collection | T1114.002 |
| Impact         | Financial Theft                           | T1657     |
