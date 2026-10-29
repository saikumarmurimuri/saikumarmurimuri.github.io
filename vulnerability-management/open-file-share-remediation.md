# Open File Share Remediation

## Issue
An open file share vulnerability occurs when a file share or network share is not properly secured and can be accessed by unauthorized users or systems. Depending on the share rights, an attacker could read or modify confidential data.

**Typical scanner finding:** the remote computer has one or more Windows shares that can be accessed over the network with the given credentials (Nessus plugin: Microsoft Windows SMB Shares Unprivileged Access, port 445/TCP).

**Applies to:** security incidents and critical vulnerabilities.

## Detection and enrichment
- The finding is raised from Tenable.sc / Nessus scan results.
- A SOAR playbook posts the scanner output into the ticket: first-seen date, plugin, port and protocol, and the list of shares that can be read.
- Tenable's own mitigation guidance is attached: restrict access by opening Explorer, right-clicking each share, going to the Sharing tab, and checking Permissions.

## Remediation steps (Windows)
1. On the host, go to the location of the file share.
2. Make sure you have the permissions needed to modify or remove the share.
3. Right-click the shared folder.
4. Select **Properties**.
5. Open the **Sharing** tab and click **Advanced Sharing**.
6. Click **Permissions**.
7. Select **Everyone**, click **Remove**, then click **Apply**.

## Validation
- Ask the asset owner to confirm the change in the ticket.
- Rescan the host and confirm the finding no longer appears. Close the ticket only on scan evidence.

## Notes
- Prioritize by data sensitivity and asset criticality, not only the severity score.
- Record the fix pattern in the knowledge base so the next analyst doesn't have to solve it again.
