# Phishing Incident Response Playbook

## Overview
Standard operating procedure for triaging, investigating, and containing phishing incidents reported via user submissions, email security gateways, or SIEM alerts.

**Tools referenced:** Trend Micro XDR, Devo SIEM/SOAR, Proofpoint, Mimecast, Microsoft 365/Entra ID, VirusTotal, MISP, AbuseIPDB

## Trigger Sources
- User-reported phishing (via "Report Phishing" button or forwarded email)
- Email security gateway alert (Proofpoint/Mimecast quarantine or detection)
- SIEM correlation rule (e.g., suspicious sender domain, known malicious IOC match)

## Investigation Steps

### 1. Initial Triage
- Review the reported email's headers, sender address, and reply-to address
- Check SPF, DKIM, and DMARC validation results
- Identify any embedded links or attachments

### 2. Enrichment
- Submit URLs/attachments to VirusTotal for reputation and detonation results
- Check sender IP/domain against threat intel sources (AlienVault OTX, MISP, AbuseIPDB)
- Search SIEM for other recipients of the same email (mass campaign vs. targeted)

### 3. Scope Assessment
- Determine how many users received the email
- Check if any users clicked the link or opened the attachment (via email gateway logs or EDR telemetry)
- If credentials may have been entered on a phishing page, check Entra ID sign-in logs for the affected user(s) for suspicious activity (impossible travel, new device, MFA prompts)

### 4. Containment
- Block sender domain/IP at the email gateway (Proofpoint/Mimecast) and firewall
- Purge the email from all mailboxes (M365 Security & Compliance / Defender)
- Block malicious URL/IOC across EDR and web proxy
- If credentials were compromised: force password reset, revoke active sessions, enforce MFA re-registration

### 5. Root Cause & Documentation
- Document timeline: initial delivery, detection, containment, resolution
- Map behavior to MITRE ATT&CK (e.g., T1566.001 – Phishing: Spearphishing Attachment / T1566.002 – Spearphishing Link)
- Update detection rules/SOAR playbook if a new pattern or gap is identified

## Escalation Criteria
Escalate to L3/IR lead if:
- Credentials were confirmed compromised and used for further access
- Malware was executed and confirmed via EDR (not just delivered)
- Incident affects a privileged/executive account
- Evidence of lateral movement or data exfiltration

## Post-Incident
- Send user awareness notification if broader campaign
- Update KB/playbook with new IOCs or lessons learned
- Track incident metrics (MTTD/MTTR) for reporting
