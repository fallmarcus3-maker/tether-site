# Tether AI — Website Agent

## What This Is

This is the marketing and landing site for **Tether**, a mobile-first workforce management app for small trade businesses (contractors, cabinet shops, landscapers, detailers). The site is a single-page HTML file hosted on **Cloudflare Pages** at **tetherapp.work**.

## Domain & Hosting

- **URL:** https://tetherapp.work/
- **Host:** Cloudflare Pages (deployed via `wrangler pages`)
- **DNS:** Cloudflare
- **Files:** Static HTML/CSS/JS — no build step, no framework

## File Structure

```
index.html          — Main landing page (single-file, self-contained CSS/JS)
tether-site.html    — Backup/working copy of the site
privacy.html        — Privacy policy
terms.html          — Terms of service
data-deletion.html  — Data deletion instructions (required by app stores)
logo.png            — Tether logo (1173 KB, used in OG tags)
images/             — Screenshot assets for the site
social/             — Social media assets
FEATURE_QUEUE.md    — Feature brief for The Queue (reference doc)
```

## The App (What the Site Promotes)

Tether is a Flutter/Dart mobile app with a Firebase backend. The full codebase lives at:
```
C:\Users\Marcus\Documents\Tether\tether\
```

### Current Version: 1.7.5

### What Tether Does

- **Radar** — Monthly calendar with color-coded task buckets, drag-to-reschedule, week/day views
- **Time Tracking** — Employee clock-in/out with lunch deduction, overtime, pay period summaries, QuickBooks IIF export
- **Office AI** — Chat assistant (Gemini-powered) that can schedule tasks, query crew status, approve time-off, and manage jobs — by text or voice
- **Proactive AI** — Morning briefs, end-of-day summaries, weekly digests, anomaly alerts (long clock-ins, overdue tasks, pending requests)
- **Crew Management** — Employee linking via Employer ID, role-based access, task assignment
- **Jobs & Templates** — Customer projects with address, notes, and reusable task-label templates
- **The Queue** — Unscheduled task pool for downtime work (one-time or recurring)
- **Home Screen Widgets** — iOS + Android native widgets showing clock-in status (employee) and crew overview (admin)
- **Geofencing** — Soft geofence that flags clock-ins outside a configurable radius
- **Google Calendar Sync** — Two-way sync with Google Calendar
- **Notifications** — Bell/drawer system with per-type toggles (task notes, time edits, time off, join requests, payroll, location flags)

### Subscription Plans

| Plan | Price (monthly) | Price (annual) | Seats | AI Queries | Features |
|------|----------------|---------------|-------|-----------|----------|
| **Solo** | $10/mo | $8/mo | 0 (admin only) | 100/mo | Standard AI model |
| **Crew** | $25/mo | $20/mo | Up to 5 | 500/mo | + Deep AI model, morning briefs, EOD, anomaly alerts |
| **Enterprise** | $60/mo | $48/mo | Up to 10 | Unlimited | Everything in Crew |

- **90-day free trial** — All features unlocked, 2 employee seats, 100 AI queries/mo
- **No credit card required** for trial
- Subscriptions managed via RevenueCat (App Store / Play Store only)

### App Store Links

- **Google Play:** https://play.google.com/store/apps/details?id=com.getorganized.tether
- **Apple App Store:** (pending / add when available)

### Target Audience

Small trade businesses: contractors, cabinet shops, woodworkers, landscapers, auto detailers, cleaning crews, HVAC, plumbing, electrical. 2-10 employees. Owner runs the crew from their phone.

## Web Portal (Coming Soon)

A Flutter web build of the same app will be deployed alongside the marketing site. The web portal is:
1. A **marketing funnel** — visitors can sign up and try Tether in the browser without downloading
2. A **convenience layer** — existing subscribers access schedules and timecards from desktop

**Billing stays mobile-only.** The web portal reads the same Firestore data, gated by the same subscription tier. "Tether is mobile-first — manage your subscription in the app."

See `PLAN_web_portal.md` in the main Tether repo for the full implementation plan.

## Design Language

- **Fonts:** Inter (body), Inter Tight (headings), Roboto Mono (labels/tags)
- **Colors:** Blue primary (`#3B5CF5`), green accent (`#22C55E`), purple secondary, dark navy text (`#1A1F2E`)
- **Style:** Clean, professional, light background (`#EEF1F8`), white cards, subtle shadows, rounded corners (16px)
- **Tone:** Direct, no-nonsense, trade-worker friendly. "Stop running your crew from memory."

## Deployment

```bash
cd "C:\Users\Marcus\Documents\Claude\Tether AI"
wrangler pages deploy . --project-name=tether-site
```

## When Updating the Site

- Keep feature descriptions in sync with what's actually shipped in the app
- Pricing must match `subscription_utils.dart` in the app repo
- Screenshots in `images/` were extracted from screen recordings — update when UI changes significantly
- The site is a single HTML file — no build step, just edit and deploy
