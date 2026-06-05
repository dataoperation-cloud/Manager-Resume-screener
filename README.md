# Warehouse Ops Screener — Deploy Guide

This is a private, password-protected hiring tool. Once deployed, anyone with the
link **and the password** can use it in a normal browser — no Claude account needed.

Your secret Anthropic API key stays on the server and is never sent to the browser.

---

## What's in this folder

```
warehouse-ops-screener/
├── index.html        ← the web page your team uses (must stay at the top level)
├── api/
│   └── analyze.js    ← the server (holds your key + the screening brief)
├── package.json
└── README.md         ← this file
```

---

## You need two things first

1. **An Anthropic API key** — get it at https://console.anthropic.com → *API Keys*.
   (API usage is billed per use; it is separate from a Claude.ai subscription.)
2. **A free Vercel account** — https://vercel.com (sign up with GitHub, GitLab, or email).

---

## Deploy in ~5 minutes (no coding)

### Option A — drag & drop (simplest)
1. Install the Vercel CLI once: open a terminal and run `npm i -g vercel`.
2. In the terminal, `cd` into this folder and run `vercel`.
3. Answer the prompts (accept the defaults). It will give you a live URL.
4. Set your secrets (see **Environment variables** below), then run `vercel --prod`.

### Option B — through GitHub (good for ongoing edits)
1. Put this folder in a GitHub repository.
2. In Vercel: **Add New… → Project → Import** your repository.
3. Leave the build settings at their defaults and click **Deploy**.

---

## Environment variables (required)

In the Vercel dashboard: **Project → Settings → Environment Variables**, add:

| Name                | Value                                   |
|---------------------|-----------------------------------------|
| `ANTHROPIC_API_KEY` | your key from console.anthropic.com     |
| `ACCESS_PASSWORD`   | a password you choose for your HR team  |

After adding them, **redeploy** (Deployments → ⋯ → Redeploy) so they take effect.

---

## Using it

1. Open the URL Vercel gave you.
2. Enter the `ACCESS_PASSWORD`.
3. Drop in resumes (PDF or .txt), click **Analyze & rank candidates**.

Share the URL + password only with the people who should have access.
To change the password later, update `ACCESS_PASSWORD` and redeploy.

---

## Editing the screening criteria

All the hiring logic (the 3-year manager rule, Kannada requirement, the 7 core
warehouse areas, scoring weights, output format) lives in **`api/analyze.js`** in
the `SYSTEM_PROMPT` text near the top. Edit that, save, and redeploy.

---

## Notes & limits

- Resumes are sent to Anthropic for analysis and are not stored by this app.
- Each resume is one API call; large batches process one at a time.
- This is a screening aid, not a hiring decision — always verify manager tenure,
  Kannada fluency, and core-ops claims directly with the candidate.
- Want it on Netlify or Cloudflare instead? The page is the same; only the
  function format differs slightly — ask and it can be adapted.
