# Phishing Incident Response Playbook

## Overview
Standard operating procedure for triaging, investigating, and containing phishing incidents reported via user submissions, email security gateways, or SIEM alerts.

**Tools referenced:** Trend Micro XDR, Devo SIEM/SOAR, Proofpoint, Mimecast, Microsoft 365 / Entra ID, VirusTotal, MISP, AbuseIPDB

## Trigger sources
- User-reported phishing (via the "Report Phishing" button or a forwarded email)
- Email security gateway alert (Proofpoint or Mimecast quarantine or detection)
- SIEM correlation rule (for example, suspicious sender domain or a known malicious IOC match)

## Investigation steps

### 1. Initial triage
- Review the reported email's headers, sender address, and reply-to address.
- Check SPF, DKIM, and DMARC validation results.
- Identify any embedded links or attachments.

### 2. Enrichment
- Submit URLs and attachments to VirusTotal for reputation and detonation results.
- Check the sender IP and domain against threat intel sources (AlienVault OTX, MISP, AbuseIPDB).
- Search the SIEM for other recipients of the same email to tell a mass campaign from a targeted one.

### 3. Scope assessment
- Determine how many users received the email.
- Check whether any user clicked the link or opened the attachment, using email gateway logs or EDR telemetry.
- If credentials may have been entered on a phishing page, check Entra ID sign-in logs for the affected users for suspicious activity: impossible travel, new device, MFA prompts.

### 4. Containment
- Block the sender domain and IP at the email gateway (Proofpoint or Mimecast) and the firewall.
- Purge the email from all mailboxes (M365 Security & Compliance or Defender).
- Block the malicious URL and IOCs across EDR and the web proxy.
- If credentials were compromised: force a password reset, revoke active sessions, and enforce MFA re-registration.

### 5. Root cause and documentation
- Document the timeline: initial delivery, detection, containment, resolution.
- Map the behavior to MITRE ATT&CK, for example T1566.001 (Spearphishing Attachment) or T1566.002 (Spearphishing Link).
- Update detection rules or the SOAR playbook if a new pattern or gap is identified.

## Escalation criteria
Escalate to L3 or the IR lead if:
- Credentials were confirmed compromised and used for further access.
- Malware was executed and confirmed through EDR, not just delivered.
- The incident affects a privileged or executive account.
- There is evidence of lateral movement or data exfiltration.

## Post-incident
- Send a user awareness notification if it was a broader campaign.
- Update the KB and playbook with new IOCs or lessons learned.
- Track incident metrics (MTTD and MTTR) for reporting.
