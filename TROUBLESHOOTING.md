# Troubleshooting - Ping on Change

Use this guide when the app isn’t behaving as expected.

## Quick checklist (covers most issues)
1. **Config on default branch?**  
   Ensure **A VALID** `workflow-state-notifications.json` exists in the **repo root** of the **default branch**.
2. **Check card visible?**  
   Open the PR/commit **Checks** tab. If you see “Configuration required,” fix the config and click **Re-check**.
3. **App installed on the right repo/org?**  
   The GitHub App must be installed on the exact account + repository.
4. **Permissions correct?**  
   App scopes: **Actions: read**, **Checks: write**, **Metadata: read**.
5. **First-failure semantics understood?**  
   You get **one** alert when a workflow **first** fails, and **one** when it **recovers** - no spam in between.
6. **Billing/suspension?**  
   If the installation is **suspended** or billing is **cancelled/paused**, the app will **skip** processing.

---

## Common problems & fixes

### 1) Nothing shows up (no Check, no email)
- **Cause:** Missing config on the **default** branch.  
- **Fix:** Commit `workflow-state-notifications.json` to the default branch → open the PR/commit **Checks** tab → click **Re-check**.

### 2) Yellow “Configuration required” won’t clear
- **Do this:** After adding/fixing the config, click **Re-check** on our Check card.  
- **Note:** The **“Resolve”** label is just a **Details** link. It **doesn’t** change state.

### 3) Where to find the Check
- PR → **Checks** tab (or the commit page). Checks don’t appear inside the Actions job log.  
- For manual runs (`workflow_dispatch`), open the **commit** referenced by the run to see Checks.

### 4) Forked PRs
- GitHub may restrict writes from forks. If our Check doesn’t appear, try a branch in the **same repo**, or merge first, then **Re-check**.

### 5) “Run skipped by app”
- We skip when:
  - Installation is **suspended**.
  - Marketplace status is **cancelled/paused/trial-expired** (or **cancel scheduled**, depending on publisher policy).  
- **Fix:** Unsuspend or restore an active plan; re-run and **Re-check**.

### 6) Billing / cancellation timing
- **Cancel/downgrade now:** GitHub schedules it.  
- **At effective date (next billing cycle), we deactivate, and data deletion begins.

### 7) Email not arriving
- **Config:** Verify `channels.email` in your JSON.  
- **Semantics:** Only **first failure** and **recovery** trigger emails.  
- **Delivery:** If you self-filter, allow messages sent via **Resend**.  
- **Hint:** Our Check **summary** often explains why no email was sent.

### 8) “Still missing/invalid config”
- Validate path + JSON (example):
  ```json
  {
    "version": 1,
    "apply_to": { "workflows": "*" },
    "channels": { "email": ["you@example.com"] }    
  }
