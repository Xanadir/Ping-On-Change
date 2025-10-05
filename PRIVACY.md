# Privacy Policy - Ping on Change

**Who we are.** Ping on Change is a GitHub App that sends first-failure and recovery notifications for GitHub Actions workflows.

## Data we process
- **GitHub metadata (from webhooks):** installation ID, account/org/repo IDs and names, workflow/run status and IDs, branch, commit SHA.
- **Configuration you add in-repo:** notification targets (e.g., email addresses) and options in your config file (e.g., `workflow-state-notifications-setup.json`).
- **Operational metadata we create:** timestamps that a notification was sent and high-level delivery diagnostics (no repository content).

**We do *not* process:** repository source code, build artifacts, or secrets.

## Purpose
Operate the app (detect first failure and recovery) and send the notifications you requested. Limited aggregate, de-identified metrics may be used to improve reliability.

## Storage & processors
- **Runtime & storage:** Cloudflare Workers and Cloudflare KV.
- **Email delivery:** **Resend** (addresses and message metadata necessary to deliver email).
All data in transit uses TLS.

## Logs and retention
- **Cloudflare Workers logs:** we do **not** retain server logs. Cloudflare’s live tail/debug logs are **ephemeral** and used only during active troubleshooting.
- **Operational metadata (our own):** minimal per-installation state in KV while your installation is active (e.g., last alert timestamps).
- **Deletion:** on GitHub Marketplace **effective cancellation** or **uninstall**, the service is deactivated immediately and personal data (configured email targets and per-installation state) is deleted within **30 days**. We keep only a minimal non-PII “tombstone” (status + timestamps) to prevent unintended reactivation.

## Your choices
- Edit or remove the repo config file to change/stop notifications.
- Uninstall the GitHub App at any time via GitHub.
