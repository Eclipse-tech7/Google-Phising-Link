# Phishing Awareness Demo

> **Educational use only.** A local Google-branded login simulation for phishing-awareness training. Not a working kit. Not for use on real people.

---

## ⚠️ Read this first

This is a **controlled training tool**. It simulates a Google sign-in and sends submitted input to a webhook **you configure yourself**, so trainees can see the "credential leaves the page" flow in a consented lab.

**You may not:**

- Point this at real people, real accounts, or a real Google service
- Use a webhook you don't own and control
- Deploy it publicly or share it as a link
- Collect, store, or retain real credentials
- Use it for scamming, fraud, or any unlawful purpose

**You must:**

- Run it only locally or on an isolated lab network
- Configure the webhook to an endpoint **you** control
- Demo with fabricated credentials only
- Get informed consent from every participant
- Clear webhook logs after each session

---

## What it does

One static HTML file. No build step, no backend, no dependencies. A draggable Google-style popup with two screens:

1. **Landing** — "Sign in," a "Sign in with Google" button, and three dead-end options ("Use another account," "Create account," "Forgot email?").
2. **Credential form** — email/phone, password, "Forgot password?", "Next."

Only "Sign in with Google" advances. Option rows, footer links, and forgot-password trigger a toast and do nothing. The "Not now"/"Back" links appear after six seconds.

On submit, the form POSTs a JSON payload to the configured webhook, spins for ~1.2s, shows a hardcoded "Wrong password" error, then redirects to the real Google sign-in page for the entered email.

### What it doesn't do

- **No default destination.** `WEBHOOK_URL` is a placeholder. Nothing sends until you set your own endpoint.
- **No backend, no storage.** Data goes only to the webhook you configure.
- **No validation.** The error message is hardcoded. Every submission behaves identically.
- **No real sign-in.** The redirect is a decoy exit. No session, no auth.
- **No 2FA, recovery, or account creation.** Those paths are dead-ends.

---

## Data sent

Exactly this, on submit:

```json
{
  "ts": "2026-…",
  "email": "<entered email>",
  "password": "<entered password>",
  "ua": "<navigator.userAgent>",
  "referrer": "<document.referrer or null>",
  "page": "<location.href>"
}
