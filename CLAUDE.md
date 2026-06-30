# Tether AI — Website Agent

> **Purpose:** marketing + landing site for Tether (`tetherapp.work`), hosted on
> Cloudflare Pages. Single-file HTML, no build step. Flutter web portal at `/app/`.

## 0. Index
1. [Workflow rules](#1-workflow-rules) — READ FIRST
2. [Memory system](#2-memory-system)
3. [Site architecture](#3-site-architecture)
4. [The app (ground truth: sync from app repo)](#4-the-app-sync-from-app-repo--may-drift)
5. [Deployment](#5-deployment)
6. [Design language](#6-design-language)

---

## 1. Workflow rules

### Deploy autonomy
- **Auto-deploy** after content edits (copy, CSS tweaks, tag fixes).
- **Ask first** for: new HTML pages, deleted pages, new JS / scripts / external deps,
  web portal rebuild (Flutter `/app/` swap).
- Command: `wrangler pages deploy . --project-name=tetherapp`

### Verification (before saying "done")
Preview server + screenshot + inspect critical elements. Use `preview_*` tools —
never Bash or Chrome MCP for dev servers.

### Git commits
Only when Marcus says "commit". Never unprompted.

### Scope
Trivial fixes (<30s) while doing other work: just do it, mention in summary.
Bigger tangential issues: flag, don't fix.

### Subagents
Use for >3 file reads or unclear scope. **Prefer Sonnet** — Haiku subagents have
hallucinated false bugs in this project.

### Cross-repo reads
The Tether app repo (`C:\Users\Marcus\Documents\Tether\tether\`) is the **source
of truth** for pricing, version, and feature copy. Read it when syncing site
content with shipped features.

### Response style
Adaptive. Terse + bulleted for edits/ships. Narrative for design/architecture
decisions. Lead with result. Don't restate the ask.

### Editing this file (CLAUDE.md)
Propose changes, wait for explicit approval. Never silently edit.

### Memory↔reality conflicts
If memory contradicts current code/files, **trust current state and
silently refresh the memory file**, then proceed.

---

## 2. Memory system

Location: `C:\Users\Marcus\.claude\projects\C--Users-Marcus-Documents-Claude-Tether-AI\memory\`
Index: `MEMORY.md` (auto-loaded). Organization: **fine-grained — one file per
concept**, indexed in `MEMORY.md`.

### Save a new memory when
- Marcus states a preference or corrects approach → **feedback** memory
- Marcus mentions a new tool / account / dashboard / ID → **reference** memory
- Marcus shares non-obvious business/project context → **project** memory
- Marcus says "save" or "remember" → save immediately, type = best fit

### Known external references (don't auto-expose; know they exist)
- **RevenueCat** — subscription backend (mobile only)
- **Mozaik / QuickBooks / ADP** — cabinet shop's existing software
- **Cloudflare Pages** — site host (fallmarcus3@gmail.com acct)
- **tetherapp.work** — domain, registered under a *different* Cloudflare account

---

## 3. Site architecture

```
index.html          — Main landing page (self-contained CSS/JS)
tether-site.html    — Working copy / backup (kept in sync)
privacy.html        — Privacy policy
terms.html          — Terms of service
data-deletion.html  — Data deletion (app store requirement)
logo.png            — 2048x2048 app icon (also used as OG image — needs compression)
favicon.png         — 32x32 favicon
apple-touch-icon.png — 180x180 iOS icon
images/             — App screenshots (JPGs, 1080x2400)
app/                — Flutter web portal build (base href patched to /app/)
```

GitHub repo: https://github.com/fallmarcus3-maker/tether-site

---

## 4. The app (sync from app repo — may drift)

**Version:** 2.7.0+42 (Google Play launched 2026-03-27)
**Target:** Small trade businesses, 2–10 employees.

Features: Radar, Time Tracking, Office AI (Gemini), Proactive AI (morning briefs,
EOD, anomalies), Crew Management, Jobs & Templates, The Queue, Home Screen
Widgets, Geofencing, Google Calendar Sync, Notifications. FCM push: Phase 1 code
complete, not yet deployed.

### Subscription plans
| Plan | Monthly | Annual | Seats | AI Queries |
|------|---------|--------|-------|-----------|
| Solo | $10 | $8 | 0 (admin only) | 100/mo |
| Crew | $25 | $20 | Up to 5 | 500/mo |
| Enterprise | $60 | $48 | Up to 10 | Unlimited |

90-day free trial, no card. Billing mobile-only (RevenueCat).

**App Store links:**
- Google Play: https://play.google.com/store/apps/details?id=com.getorganized.tether
- Apple: build on TestFlight (2026-05-30, passed Apple's Xcode 26 / iOS 26 SDK
  validation); on-device testing + App Store listing/metadata + submission
  remain. Code is latest-Xcode-ready. iOS builds run via Codemagic (Mac CI).

---

## 5. Deployment

### Marketing site
```bash
cd "C:\Users\Marcus\Documents\Claude\Tether AI"
wrangler pages deploy . --project-name=tetherapp
```

### Web portal (Flutter web at `/app/`)
```bash
cd "C:\Users\Marcus\Documents\Tether\tether"
flutter build web --release    # no --base-href on Windows (SDK path has a space)
cd "C:\Users\Marcus\Documents\Claude\Tether AI"
rm -rf app/ && cp -r "C:\Users\Marcus\Documents\Tether\tether\build\web" ./app
sed -i 's|<base href="/">|<base href="/app/">|' app/index.html    # CRITICAL
wrangler pages deploy . --project-name=tetherapp
```

---

## 6. Design language

- Fonts: Inter (body + headings) — only font loaded in index.html.
- Colors: `#3B5CF5` primary, `#6366F1` purple, `#22C55E` green, bg `#EEF1F8`,
  surface `#FFFFFF`, dark text `#1A1F2E`.
- Style: light theme, white cards, 16px radius, subtle shadows.
- Tone: direct, trade-worker friendly. "Stop running your crew from memory."
