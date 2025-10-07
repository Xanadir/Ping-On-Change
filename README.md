## 🗝️ Key features
- **Status-change only:** fail → ping, recover → ping. No repeats while a workflow stays red.
- **Indie-friendly setup:** add a tiny JSON file; no secrets or extra infra required.
- **Multiple recipients per payer:** one person pays, notify many.
- **Minimal permissions:** Actions (read) + Metadata (read) only.

---

## 💪 How it works
1. **Install** the app to your user/org and choose repos.  
2. **Configure** each repo you want monitored by adding a JSON file (see below).  
3. **Relax.** We watch workflow results and email **once** on first failure, **once** on recovery.

---

## ⚙️ Required configuration (per repo)
Create `workflow-state-notifications.json` at the repo root on the **default branch**:

```json
{
  "version": 1,
  "apply_to": {
    "workflows": [".github/workflows/nightly-ci.yml", ".github/workflows/healthcheck-ci.yml"],    
  },
  "channels": {
    "email": ["dev@awesomo.com", "designer@awesomo.com", "qa@awesomo.com"]
  }
}
```
> Only repos containing this file are monitored.  

---

## 🧰 Perfect for
- Solo maintainers and small teams who want **signal, not noise**
- OSS projects that need a simple **first-red / back-to-green** ping
- Side projects with nightly/scheduled CI

---

## ❓ FAQ
**Does it alert on every failed run?**  
No. You are only notified on **first failure** and **recovery**, e.g. status changes.

**Org vs personal repos?**  
Both are supported. The billing account (user or org) controls entitlements.

**What if there’s no config file in a repo?**  
We ignore it to keep noise and cost low.

---

## 🆘 Support
Questions, bugs, or ideas? Email **[xanadirstrangebird@gmail.com](mailto:xanadirstrangebird@gmail.com)** or open an issue at **[https://github.com/Xanadir/Ping-On-Change/issues](https://github.com/Xanadir/Ping-On-Change/issues)**. We welcome suggestions, feature requests, and clear repro steps to help us fix things fast.

