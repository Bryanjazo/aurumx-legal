# AurumX legal site

Static Jekyll site that hosts the four legal documents AurumX needs to be live before any App Review pass:

- [Privacy Policy](privacy.md)
- [Terms of Service](terms.md)
- [Responsible Play](responsible-play.md)
- [Open-Source Licenses](licenses.md)

The four markdown files are first-pass drafts based on what the app actually does. **Every page needs a legal review before publishing** — look for the `⚠️ LEGAL REVIEW REQUIRED` callouts at the top of each.

## Publishing checklist

### 1. Register the domain
- Buy `aurumxgold.app` at a registrar (Cloudflare, Namecheap, Porkbun, etc.). About $12-15/yr.

### 2. Choose a hosting layout

You have two reasonable options for putting this `legal/` directory on the web:

**Option A — Pages from a subdirectory of the AurumX repo** (if this repo is or becomes public)
- Repo Settings → Pages → Source: "Deploy from a branch" → Branch: `main` → Folder: `/legal`.

**Option B — Pages from a separate dedicated repo** (recommended; lets the AurumX app repo stay private)
- Create a new public repo `aurumx-legal`.
- Copy the contents of this `legal/` directory to the root of that repo (so `_config.yml` and the `.md` files sit at the root, not inside `legal/`).
- That repo's `main` branch is your Pages source.

### 3. Point the domain at GitHub Pages

Two DNS records at your registrar:

```
A    aurumxgold.app    185.199.108.153
A    aurumxgold.app    185.199.109.153
A    aurumxgold.app    185.199.110.153
A    aurumxgold.app    185.199.111.153
```

(Or a single CNAME `aurumxgold.app` → `<username>.github.io` if your registrar supports CNAME on the apex.)

### 4. Tell GitHub the domain

- The repo serving Pages → Settings → Pages → **Custom domain** → `aurumxgold.app` → Save.
- Wait a few minutes for Let's Encrypt to issue the cert.
- Tick **Enforce HTTPS**.

### 5. Verify the four URLs load

```
https://aurumxgold.app/         → index
https://aurumxgold.app/privacy
https://aurumxgold.app/terms
https://aurumxgold.app/responsible-play
https://aurumxgold.app/licenses
```

Each one should render readable HTML with no auth required.

### 6. Paste the privacy URL into App Store Connect

App Store Connect → your app → **App Privacy** → **Privacy Policy URL** → `https://aurumxgold.app/privacy` → Save.

Apple's reviewer will click this on every submission. If it 404s or returns a malformed page, your build gets rejected immediately.

## Editing the docs

Each `.md` file is plain markdown with a Jekyll front-matter block at the top:

```markdown
---
title: Privacy Policy
permalink: /privacy
---
```

`permalink` controls the public URL. Don't rename `permalink` after submitting to Apple — the URL is canonical.

Push to the Pages branch → GitHub Actions rebuilds in ~30 seconds → the new content is live.

## Why this exists in the AurumX repo at all

So the drafts ship with the rest of the work and the four URLs the app links to (`Account → Legal & Privacy`) can be kept in sync as the documents evolve. Once published you may want to move it to a dedicated repo, but starting it here keeps reviews tight.
