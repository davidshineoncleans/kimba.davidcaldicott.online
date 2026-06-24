# kimba.davidcaldicott.online

KIMBA landing / sales page. Static HTML, no build step. Mirrors the `connect-deploy` setup.

- `index.html` — the whole page (built from `KIMBA/Landing-Page/kimba-page_v1.html`; **Sanctuary is the source of truth**, this repo is the deploy mirror).
- Images live at **repo root** (flat): `kimba-hero.jpg` (B&W woodland), `kimba-tree.jpg` (pinetree), `kimba-cathedral.jpg`, `kimba-arch.jpg`.
- Hosting: Vercel (static, no build).
- Note: the Sanctuary source uses an `assets/` subfolder; the live repo is flat, so when re-copying, rewrite `assets/kimba-` → `kimba-` in the image paths (one sed).

## Deploy flow

1. Edit in Sanctuary (`Spaces/Work & Ventures/KIMBA/Landing-Page/kimba-page_vN.html`).
2. Copy the current version to `index.html` here; copy any new images into `assets/`.
3. Commit + push → Vercel auto-deploys. Point the `kimba` subdomain (CNAME) at the Vercel project.

## Config

All runtime config is in the `CONFIG` object at the top of the `<script>` block in `index.html`:
`calBase` · `calDiscovery` · `meetUrl` · `captureWebhook` · `starterSource` · `page`.

## CTAs / wiring

- **Primary — Starter Pack opt-in** (`#starter`): the email form POSTs `{email, source:'kimba-starterpack', page:'kimba'}` to `captureWebhook` (n8n `cyd-capture`). It reuses the CYD capture endpoint with a distinct `source` so KIMBA leads are taggable.
- **Secondary — discovery call** (`#conversation`): links to `meet.davidcaldicott.online` (current discovery destination). To skip the meet page and go straight to Cal, uncomment the `discovery-link.href` line in the script.

## Gates — do NOT publish publicly until clear

- 🚦 **CORS:** add `https://kimba.davidcaldicott.online` to the `cyd-capture` webhook's `allowedOrigins` (the *subdomain*, not the apex) — this exact omission broke the CYD form on 2026-06-17.
- 🚦 **Delivery:** confirm the Sequenzy automation that auto-emails `KIMBA-Starter-Pack.pdf` fires on `source=kimba-starterpack` (Route A in `lead-magnet-capture-delivery_PLAN`). Until wired, the form shows the thank-you but no PDF sends.
- 🚦 **PDF host:** `KIMBA-Starter-Pack.pdf` uploaded to the public `images_public` bucket and linked in the delivery email.
- 🚦 **Imagery:** confirm David is happy with the candid practice frames going public (they show him mid-practice in a park).
- 🚦 **Discovery slug:** confirm `cal.com/davidcaldicott/discovery` is the right destination, or keep the `meet.` page.
