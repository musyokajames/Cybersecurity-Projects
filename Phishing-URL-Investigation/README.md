# Phishing Investigation: URL Infrastructure & Evasion Analysis

## Overview

This project focuses on the analysis of a live phishing URL. When an email bypasses header authentication, analysts must pivot to the embedded links. The objective was to use dynamic analysis (sandboxing) to safely observe the destination website, identify the infrastructure hosting the attack, and document the evasion techniques used by the threat actor.

## The Evidence

- **Artifact:** URL analysis report hosted on urlscan.io.
- **Tools Used:** urlscan.io (Dynamic Sandbox), AI Assistant.

## The Scenario

A user reported an email claiming to be a secure DocuSign document from TD Bank. The link did not go to `docusign.com`. Instead, it leveraged a typosquatting domain designed to steal banking credentials.

## Investigation Walkthrough

### Phase 1: Dynamic Analysis via Sandbox

Using urlscan.io, the malicious URL (`docusign-tdbank.com.ph`) was safely detonated in a cloud sandbox.

- **The Finding:** The site was hosting a pixel-perfect replica of a TD Bank login portal, branded with DocuSign logos to appear legitimate.
- **Infrastructure:** The scan revealed the site was hosted on an IP address (`45.79.222.138`) running an `openresty` web server.

![URLScan Screenshot](screenshots/01-urlscan-screenshot.png)

_The urlscan.io capture of the fake DocuSign/TD Bank login page._

### Phase 2: Infrastructure Analysis (HTTP Transactions)

By reviewing the HTTP transaction logs within urlscan.io, we observed how the page loaded. Attackers often use redirects or cloaking to hide the final payload from email security gateways.

![Redirects](screenshots/02-redirects.png)

_Analyzing the server response and HTTP transaction details._

### Phase 3: The AI Analysis & Limitations

The URL was submitted to an AI assistant to evaluate its threat intelligence capabilities.

![AI URL Analysis](screenshots/03-ai-url-analysis.png)

_The AI analyzing the malicious URL._

**Where AI Succeeded:** The AI correctly recognized that the domain `docusign-tdbank.com.ph` was a typosquatting attempt combining two trusted brand names.

**Where AI Failed:** The AI explicitly stated it could not browse the live internet. It could not confirm if the site was currently active, what it looked like, or what IP address was hosting it. Therefore, AI cannot replace dynamic sandboxing tools like urlscan.io for real-time verdicts.

## Lessons Learned

- **Typosquatting is obvious to humans, invisible to machines:** Merging two brand names (`docusign` + `tdbank`) is a common trick that fools basic filters.
- **Never visit the link:** Dynamic analysis tools (sandboxes) are mandatory. Visiting a suspected phishing link on a corporate workstation risks drive-by downloads or IP logging.
- **Defanging URLs:** When documenting IOCs, analysts should "defang" URLs to prevent accidental clicks (e.g., `hxxps://docusign-tdbank[.]com[.]ph`).
