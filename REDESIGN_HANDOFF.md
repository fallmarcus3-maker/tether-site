# Tether Website — Redesign Handoff Brief

**For:** a design-focused Claude (Fable) pass on the Tether marketing site.
**From:** prior working session, 2026-07-03.
**Repo:** `C:\Users\Marcus\Documents\Claude\Tether AI` · live at **tetherapp.work** (Cloudflare Pages).

---

## 1. The ask (what Marcus wants)

1. **Make it look more official / more credible** — it currently reads a bit like a
   one-person indie landing page. Raise the trust and polish without losing the voice.
2. **Break the single long page into a proper multi-page site** — the whole site is
   one ~2,200-line `index.html` right now. Split it into real pages with clean nav.
3. **Keep the support page URL intact** (see §3 — this is a hard constraint).

"Official" here means **credible and trustworthy, not stiff/corporate.** Keep the
trade-worker voice (see §5). The goal is "a real company builds this," not "enterprise SaaS."

---

## 2. Current state (what exists)

```
index.html          — entire marketing site, single self-contained file (~2,200 lines,
                      inline CSS + JS). Sections, in order:
                        nav · hero · #problem · #features · screenshots-scroll ·
                        #how-it-works · #employees · #pricing · #reviews · #faq · footer
tether-site.html    — working backup / mirror of index.html (kept in sync by hand)
privacy.html        — Privacy Policy   (URL registered with app stores)
terms.html          — Terms of Service
data-deletion.html  — Data Deletion    (app-store requirement — URL registered)
app/                — Flutter web portal (login app). DO NOT TOUCH. Separate base href.
images/             — app screenshots: screen-today, screen-officeai,
                      screen-radar-month, screen-office, screen-vault (phone JPGs)
logo.png (2048²) · favicon.png (32²) · apple-touch-icon.png (180²)
```

Footer links today: Features (#), Pricing (#), **Support → `mailto:terry.tetherapp@gmail.com`**,
Privacy (privacy.html), Terms (terms.html), Data Deletion (data-deletion.html).
Nav (desktop + mobile hamburger): Features / Pricing / FAQ (all in-page anchors) + a
"Open the app" button → `/app/`.

---

## 3. Hard constraints — DO NOT BREAK

- **Buildless.** No framework, bundler, npm, or build step. Every page is a single
  self-contained `.html` with inline (or `<style>`-block) CSS and inline JS. This is a
  firm requirement of the hosting setup — keep it that way.
- **`/app/` is off-limits.** It's a separately-built Flutter web portal with its own
  `<base href="/app/">`. Don't edit, move, or link over it. The "Open the app" button
  must keep pointing at `/app/`.
- **Frozen URLs** — these paths are registered with Apple/Google and/or linked
  externally. They MUST keep resolving at the exact same path after the redesign:
  - `/` (homepage)
  - `/privacy.html`
  - `/terms.html`
  - `/data-deletion.html`
  - **The Support URL registered with the app stores.** ⚠️ Right now "Support" is only a
    `mailto:`, and there is **no `support.html`**. Before renaming/moving anything,
    CONFIRM with Marcus what URL is registered as the app's Support URL in App Store
    Connect / Play Console (likely the homepage). Whatever it is, it must keep working.
    You MAY add a real Support/Contact page (see §4) — just don't change or break the
    already-registered URL. If you add `support.html`, keep the existing registered URL
    resolving (same path, or a redirect Cloudflare can honor).
- **One font: Inter.** It's the only font loaded. Keep it single-font.
- **Deploy is manual and gated.** `wrangler pages deploy . --project-name=tetherapp`.
  New pages / structural changes require Marcus's OK before deploying (per repo rules).

---

## 4. Suggested structure (multi-page split)

Turn the one-pager into these, sharing a consistent header + footer across all:

- **Home `/` (index.html)** — tighten to: hero → the problem → a *condensed* features
  overview (3–4 highlights, link to full Features page) → social proof → primary CTA.
  Don't dump every feature here anymore.
- **Features `/features.html`** — the deep feature sections live here. This is where the
  product's real depth should shine: Office AI, time tracking, crew, Queue, Board, Vault.
- **Pricing `/pricing.html`** — plans + monthly/annual toggle + billing FAQ.
- **Support / Contact `/support.html`** *(new, optional but recommended for "official")* —
  a proper page: contact, top FAQs, and links to Privacy / Terms / Data Deletion. Replaces
  the bare `mailto:`. ⚠️ Only add this if it does not conflict with the registered Support
  URL (§3) — coordinate with Marcus.
- **Keep** privacy / terms / data-deletion as separate pages (URLs frozen). Fine to
  restyle them to match the new look, but the paths stay.

Because there's no build step, the **shared header and footer are copy-pasted** into each
page — keep them byte-identical so the site feels like one thing. Update nav to route to
the new pages; keep the mobile hamburger.

**Polish checklist for "more official":**
- Per-page `<title>` + `<meta name="description">`; Open Graph/Twitter meta + OG image.
- Consistent spacing scale and type rhythm; align the card/shadow/radius system.
- Real footer with company/contact identity and a consistent legal-links row on every page.
- Favicon + apple-touch-icon wired on every page.
- A trust band somewhere (app-store badges, "on Google Play", 90-day trial, privacy stance).

---

## 5. Design language (keep / evolve)

- **Fonts:** Inter (headings + body) — the only font loaded.
- **Colors:** `#3B5CF5` primary blue · `#6366F1` purple · `#22C55E` green ·
  bg `#EEF1F8` · surface `#FFFFFF` · dark text `#1A1F2E`.
- **Style:** light theme, white cards, ~16px radius, subtle shadows.
- **Voice:** direct, trade-worker friendly. Anchor line: *"Stop running your crew from
  memory."* Also on-brand: *"Replace the whiteboard and the group text."* Keep this tone —
  making it "official" must not sand off the plainspoken edge.

You have latitude to modernize the visual system (better hierarchy, spacing, sectioning),
but stay within this palette/voice unless proposing a deliberate refresh to Marcus first.

---

## 6. Content truth — guardrails (don't drift from the app)

Match what the app actually does. Don't invent features or oversell.

- **Real features:** Radar/schedule, Time Tracking (shop time vs site time), **Office AI**
  (focused modes, reads real business data, proactive morning/EOD briefings) — this is the
  differentiator, lead with it — Crew Management, Jobs & Templates, **The Queue**,
  **The Board**, **Document Vault** (searchable, incl. text inside files), Google Calendar
  sync, notifications.
- **Do NOT brag about the scheduler** — it needs work. Present it modestly, not as a headline.
- **No "receipts" feature** — don't imply expense/receipt capture.
- **Pricing:** Solo $10/mo ($8 annual) · Crew $25/$20 · Enterprise $60/$48 ·
  **90-day free trial, no credit card** · billing is mobile-only.
- **Availability:** live on Google Play; iOS in App Store submission.
- Portal login lives at `/app/`.

---

## 7. Workflow for the redesign

1. Work in `index.html` + new page files; keep `tether-site.html` mirror in sync.
2. Preview + screenshot + inspect before calling anything done (use `preview_*` tools,
   never Bash/Chrome for dev servers).
3. Verify every frozen URL from §3 still resolves.
4. Get Marcus's sign-off before deploying new/deleted pages, then:
   `wrangler pages deploy . --project-name=tetherapp`

## 8. Open item to confirm with Marcus before starting

- ~~**Exact Support URL** registered in App Store Connect / Play Console~~ **RESOLVED
  2026-07-03:** Marcus confirmed the URL registered with the stores is
  `tetherapp.work/privacy` (he didn't mean a separate support URL when writing this
  brief). `/privacy.html` is already on the frozen list in §3, so no new constraint.
  Marcus approved adding a new `support.html` and chose the "push further" design scope
  (bolder visual system, direction shown on the homepage first before rolling out).
