# Troubleshooting - Ping on Change

Use this guide when the app isn’t behaving as expected.

## Quick checklist (covers most issues)
1. **Config on default branch?**  
   Ensure **A VALID** `workflow-state-notifications.json` exists in the **repo root** of the **default branch**.
2. **App installed on the right repo/org?**  
   The GitHub App must be installed on the exact account + repository.
3. **Permissions correct?**  
   App scopes: **Actions: read**, **Metadata: read**.
4. **First-failure semantics understood?**  
   You get **one** alert when a workflow **first** fails, and **one** when it **recovers** - no spam in between.
5. **Billing/suspension?**  
   If the installation is **suspended** or billing is **cancelled/paused**, the app will **skip** processing.

---

## Common problems & fixes

### 1) Nothing shows up (no email)
- **Cause:** Missing config on the **default** branch.  
- **Fix:** Commit a valid and correct `workflow-state-notifications.json` to the default branch.

### 2) “Run skipped by app”
- We skip when:
  - Installation is **suspended**.
  - Marketplace status is **cancelled/paused/trial-expired** (or **cancel scheduled**, depending on publisher policy).  
- **Fix:** Unsuspend or restore an active plan; re-run.

### 3) Billing / cancellation timing
- **Cancel/downgrade now:** GitHub schedules it.  
- **At effective date (next billing cycle), we deactivate, and data deletion begins.

### 4) Email not arriving
- **Config:** Verify `channels.email` in your JSON.  
- **Semantics:** Only **first failure** and **recovery** trigger emails.  
- **Delivery:** If you self-filter, allow messages sent via **Resend**.

### 5) “Still missing/invalid config”
- Validate path + JSON (example):
  ```json
  {
    "version": 1,
    "apply_to": { "workflows": "*" },
    "channels": { "email": ["you@example.com"] }    
  }
