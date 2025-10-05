# Setup — Ping on Change

This guide explains exactly how to configure the app and how the code interprets your config. Follow the order below.

---

## 1) Required config file (name & location)

- **File name:** `workflow-state-notifications-setup.json`
- **Location:** repository **root** of the **default branch** (usually `main`)
- If the file is **missing** or **invalid JSON**, the app **skips** processing (and, if Checks are enabled, may show a “Configuration required” card).

**Minimal working example**

```json
{
  "version": 1,
  "apply_to": { "workflows": "*" },
  "channels": { "email": ["you@example.com"] }
}
```

---

## 2) Workflow selection (`apply_to.workflows`) and `*` behavior

The app decides whether the **current workflow run** is in scope using `apply_to.workflows`:

- `"*"` → **all workflows** in the repo are eligible, then capped by your plan’s **workflows-per-repo** limit.
- `[".github/workflows/ci.yml", ".github/workflows/release.yml"]` → only those paths, taken **in listed order** and then capped by the plan.
- `[".github/workflows/*-ci.yml"]` → glob match on workflow **paths** (e.g., `.github/workflows/build-ci.yml`).

> Matching is against the workflow **file path** (like `.github/workflows/build.yml`).

**If the current run’s workflow is not inside the selected/capped set, the app skips** with a reason similar to `workflow_over_limit_or_does_not_exist`.

---

## 3) Truncation / caps (plan limits)

The app applies caps in two places:

- **Workflows per repo**
  - With `"*"`: it fetches available workflows and takes the **newest N** (by creation time) up to your plan’s limit.
  - With an **array**: it takes the **first N** entries in your list.
- **Recipients per repo**
  - Only the **first N** emails in `channels.email` are used.

> If, after capping, there are **zero recipients**, the app skips with `no_recipients`.

---

## 4) When notifications fire (first-failure / recovery)

Per **repo + workflow + branch** the app sends:

- **First failure:** once when the latest terminal result changes from success → failure.
- **Recovery:** once when it changes from failure → success.

There are **no emails** for repeated failures/successes in a row. Non-terminal/in-progress states are ignored. Duplicate sends are avoided per run/attempt.

---

## 5) Permissions & optional UI

**Required for core logic**

- `Actions: read`
- `Metadata: read`

**Optional (nice to have)**

- `Checks: write` — enables a rich **Check Run** card on PR/commit when configuration is missing/invalid.
  - If not granted, the app simply **doesn’t** post Checks (no breakage).
  - You can add this permission later; existing installs continue to work without it.

---

## 6) Other behavior worth knowing

- **Billing/suspension gate:** if your installation is **suspended** or billing is **cancelled/paused/trial‑expired**, the app **skips** processing.
- **Config changes:** commit updates to the **default branch** so new runs pick them up.
- **No repository code access:** the app reads workflow/event metadata and your config file only.

---

## 7) Copy‑paste samples

**All workflows (then capped by plan)**

```json
{
  "version": 1,
  "apply_to": { "workflows": "*" },
  "channels": { "email": ["you@example.com"] }
}
```

**Specific workflows (order matters; first N are used)**

```json
{
  "version": 1,
  "apply_to": {
    "workflows": [
      ".github/workflows/ci.yml",
      ".github/workflows/release.yml"
    ]
  },
  "channels": { "email": ["you@example.com", "qa@example.com"] }
}
```

**Glob example**

```json
{
  "version": 1,
  "apply_to": { "workflows": [".github/workflows/*-ci.yml"] },
  "channels": { "email": ["you@example.com"] }
}
```

---

## 8) Summary

1) Place **`workflow-state-notifications-setup.json`** in the **root** of the **default branch**.  
2) Use `apply_to.workflows` to select runs (`"*"` = all, or a list/glob), knowing caps will truncate.  
3) Provide emails under `channels.email`; only the first N are used.  
4) Expect **one** email on **first failure** and **one** on **recovery** per workflow/branch.  
5) Optional: grant **Checks: write** for a richer PR/commit card—safe to add at any time.
