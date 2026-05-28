# FTH Platform Reference — Toastmasters Pathways App

**Researched:** 2026-05-28
**Gate test status:** PASSED — confirmed 2026-05-28 at https://4039.toastmastersclubs.org/fth-test.html

---

## What is FTH?

Free Toast Host (FTH) is the official website platform provided to all Toastmasters clubs.
Each club gets a subdomain: `clubname.toastmastersclubs.org`.

The club's FTH site has three fixed zones:

```
+------------------------------------------+
|       HEADER (FTH-managed)               |
+----------+-------------------------------+
|          |                               |
| SIDEBAR  |   HOMEPAGE / CONTENT (editable)|
| (FTH)    |   <-- either CMS paste         |
|          |   or a linked static file      |
+----------+-------------------------------+
```

- **Header**: Managed by FTH. Shows club name and TI banner. Not editable via HTML.
- **Sidebar**: Managed by FTH. Contains navigation, login, member-only sections.
- **Content area**: Editable. Two modes — see below.

---

## Two Deployment Modes — Critical Distinction

There are two different ways HTML gets into FTH. They have very different constraints.

### Mode A — CMS Editor Paste (homepage fragments)

Used for: Club homepage content, news sections, meeting schedules.

**How:** Admin panel → Site Administration → Main Heading → Source (`<>`) button → paste HTML → Save.

**Constraints:**
- The HTML is **injected into the FTH page body** — not a standalone document. Do NOT use `<html>`, `<head>`, or `<body>` tags.
- `<style>` tags with embedded CSS are honored.
- `<link>` tags for Google Fonts work.
- JavaScript support is very limited. Complex scripts or frameworks will hang or fail.
- CSS class names should be prefixed with `fth-` to avoid conflicts with FTH's own stylesheet (e.g., `.fth-hero`, `.fth-card`).
- Images: upload via FTH media manager first, reference the resulting URL.

**Example structure:**
```html
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap" rel="stylesheet">
<style>
  .fth-hero { background: #004165; color: white; padding: 40px; }
</style>
<div class="fth-hero">
  <h1>Club Name</h1>
</div>
<div class="fth-disclaimer">
  The information on this website is for the sole use of Toastmasters' members...
</div>
```

### Mode B — File Management Static Upload (this project's mode)

Used for: **The Pathways app (`pathways-app.html`)** — and any other self-contained tools.

**How:** Admin panel → File Management → Upload file → access via direct URL.

**URL pattern:** `https://4039.toastmastersclubs.org/filename.html`

**Constraints:**
- The file is served as a **regular static HTML file** by FTH's web server.
- Full `<!DOCTYPE html>` document structure required (`<html>`, `<head>`, `<body>`).
- **Full JavaScript is supported** — inline `<script>` tags, ES2020, DOM manipulation, IntersectionObserver, etc. (Confirmed by gate test — see below.)
- All CSS and JS must be embedded (no separate `.css` or `.js` files — FTH has no way to link them).
- External CDN links work: Google Fonts `@import`, `<link rel="preconnect">`, etc.
- Max file size: 65MB (effectively unlimited for an HTML file).
- No server-side processing — static delivery only.

---

## FTH Gate Test — PASSED

Before writing any real code, a throwaway test file was uploaded to confirm FTH's static
file delivery preserves inline `<script>` tags.

**Test file:** `fth-test.html` (in repo root)
**Uploaded via:** File Management (NOT the CMS editor)
**Live URL:** https://4039.toastmastersclubs.org/fth-test.html
**Result:** Page displayed "JavaScript ran successfully. Script tags are allowed." — PASS
**Confirmed:** 2026-05-28

This test gates the entire architecture. The Pathways app depends on `<script>` tags
working in static file uploads. If a future FTH migration breaks this, re-run the test
before debugging any JS issues.

---

## How to Upload Static Files to FTH

1. Log in to the FTH admin panel at `4039.toastmastersclubs.org`
2. Navigate to **File Management** (NOT the Pages/Content/CMS editor — that strips script tags)
3. Click **Upload**
4. Select the `.html` file
5. After upload, the file is immediately accessible at `https://4039.toastmastersclubs.org/{filename}.html`
6. No cache busting needed — FTH serves the current uploaded file on each request

**Important:** The CMS page editor (`Site Administration → Main Heading → Source`) is a different
path and strips script tags. Always use File Management for the Pathways app uploads.

---

## Pathways App and FTH — Architecture Summary

The Pathways app is a **Mode B** (static file) deployment:

| Property | Value |
|---|---|
| Deployment mode | File Management static upload |
| URL | https://4039.toastmastersclubs.org/pathways-app.html |
| Document structure | Full `<!DOCTYPE html>` with `<html>/<head>/<body>` |
| JavaScript | Full ES2020 — no limitations |
| CSS | All embedded in `<style>` in `<head>` |
| External dependencies | Google Fonts only (CDN, network-dependent) |
| FTH CSS prefix | Not needed — file is isolated, not injected into FTH page |
| Max file size | 65MB |

The app does NOT appear inside the FTH sidebar/header chrome — it is a standalone page.
Club members navigate to it directly or via a link in the FTH homepage.

---

## Brand Compliance

Whether using Mode A or Mode B, all club web content must comply with Toastmasters
International brand guidelines:

| Token | Value | Usage |
|---|---|---|
| Loyal Blue | `#004165` | Headers, primary backgrounds, Required badge bg |
| True Maroon | `#772432` | Accent backgrounds, secondary headers |
| Cool Gray | `#A9B2B1` | Neutral areas |
| Happy Yellow | `#F2DF74` | Elective badge bg ONLY — black text; NEVER on light backgrounds (fails WCAG 1.4:1) |
| Headline font | Montserrat (Google Fonts) | All headings |
| Body font | Source Sans 3 (Google Fonts) | Body text |

**Mandatory footer disclaimer** (required on every club page):
> "The information on this website is for the sole use of Toastmasters' members, for Toastmasters business only."

---

## External Reference Repo

The `Bond7010/toastmasters-fth-homepage` repo contains:
- A Claude Code skill for generating FTH **homepage** HTML (Mode A fragments)
- A standalone HTML builder for non-technical club officers
- Brand guidelines and FTH constraints reference

That repo targets **Mode A** (CMS paste). The Pathways app is **Mode B** (static file).
The two deployment modes have different rules — do not conflate them.
