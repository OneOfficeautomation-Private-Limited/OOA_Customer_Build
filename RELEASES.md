# Releases

Newest first. Download each release from [Releases](../../releases).

## 0.1.1 (2026-10-26)

- **A licensed installation runs on Microsoft SQL Server.** The 30-day evaluation still runs on the built-in database.
  Installing a license requires SQL Server first; the Licenses page shows the steps (`bin\ooa.bat db configure`,
  `set-password`, `init`, `copy`), and the copy step keeps the evaluation's data. See INSTALL.md, section 5.
- The README inside the zip and INSTALL.md describe the new order: evaluate, move to SQL Server, then license.

## 0.1.0 (2026-10-26)

First customer release.

- **One Command Center:** agent repository, runs, runners and runner pools, schedules, triggers, work queues,
  workflows, the vault, users, roles, teams and the audit trail. Pages show live status and counts.
- **One Agent Studio:** about 1,000 commands in toolkits for Excel, Word, PDF, email, browser, desktop, databases,
  REST, Microsoft 365, SAP, file transfer, AI and more.
- **Databases:** built-in for evaluation, and Microsoft SQL Server for production, with separate setup and data-only
  accounts and encrypted connections.
- **Licensing:** signed license files with Developer, Citizen Developer, Attended Runner and Unattended Runner seats;
  a 30-day evaluation without a license.
- **Security:** checked settings, rate limits, an IP allow list, audit forwarding to a SIEM over syslog, a readiness
  check, password rules and session limits.
