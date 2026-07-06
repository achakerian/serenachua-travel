# Serena Chua Travel — website wireframe

A **mid-fidelity wireframe** for an independent (contractor) leisure travel agent
just starting out. Plain static HTML + one CSS file — no build step, no framework
— so it hosts on **GitHub Pages** as-is.

> ⚠️ This is a wireframe: placeholder copy, gray image blocks, and `[TBC]`
> markers throughout. Nothing here is final content. Work through the checklist
> below to turn it into a live site.

## Pages

| File | Page | Purpose |
|------|------|---------|
| `index.html` | Home | Hero, why-book-independent, featured packages, about teaser, testimonials |
| `about.html` | About | Serena's story, credentials, quick facts |
| `services.html` | Services | Service categories, "how it works", pricing model |
| `packages.html` | Packages | Ready-to-book trips with **Book & pay** buttons + a bespoke-enquiry card |
| `contact.html` | Contact | Enquiry form (Formspree-ready) + contact details |
| `styles.css` | — | Shared stylesheet. Design tokens (colours, spacing) live in `:root` at the top |

## Preview locally

Just open `index.html` in a browser — or serve the folder:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Deploy to GitHub Pages

1. Create a repo (e.g. `serenachua-travel`) and push these files to it.
2. Repo **Settings → Pages → Build and deployment**.
3. Source: **Deploy from a branch**, branch **main**, folder **/(root)**.
4. Save. Your site appears at `https://<username>.github.io/serenachua-travel/`.
5. (Optional) add a custom domain under the same Pages settings.

Because every page links with relative paths (`about.html`, `styles.css`), it
works under a project sub-path without changes.

## Payments (important: GitHub Pages has no backend)

GitHub Pages serves static files only — it **cannot process payments itself**.
The "Book & pay deposit" buttons on `packages.html` are placeholders. To take
money without a server, use a **hosted checkout link**:

1. In **Stripe** create a *Payment Link* (Dashboard → Payment Links), or make a
   **PayPal** button / PayPal.me link.
2. Copy the URL.
3. In `packages.html`, replace every `href="#REPLACE_WITH_PAYMENT_LINK"` with it.
4. Remove the `btn--pay` class from that link (it's what shows the yellow
   "link TBD" badge).

Clicking the button then sends the client to Stripe/PayPal's own secure checkout
page — no backend needed. For fully custom trips, clients use the enquiry form
and you send a deposit link or invoice manually.

## Enquiry form (needs a form service)

Static sites can't email form submissions on their own. The form in
`contact.html` is pre-wired for **Formspree**:

1. Sign up at <https://formspree.io> (free tier) and create a form.
2. Replace `YOUR_FORM_ID` in the `<form action="…">` with your endpoint.
3. Optional: uncomment the `_next` hidden field to redirect to a thank-you page.

Alternatives that work the same way: Netlify Forms, Getform, Web3Forms.

## Placeholder checklist (things to replace before launch)

- [ ] **Logo** — the `LOGO` box in the header/footer
- [ ] **Brand colours & fonts** — edit the `:root` tokens in `styles.css`
- [ ] **Hero + section images** — every `IMAGE / dimension` gray block
- [ ] **Serena's bio & portrait** — `about.html`
- [ ] **Real copy** — anything marked with the dashed `[TBC]` style or "Placeholder"
- [ ] **Credentials** — ATOL / ABTA / IATA / host agency badges & membership numbers
- [ ] **Contact details** — email, phone/WhatsApp, hours, location, socials
- [ ] **Packages** — real names, prices, deposits, inclusions, images
- [ ] **Payment links** — Stripe/PayPal on each package (see above)
- [ ] **Form endpoint** — Formspree ID (see above)
- [ ] **Legal** — privacy policy link, booking terms, company/registration details
- [ ] **Remove the yellow wireframe banner** — delete the `.wf-banner` div from each page

## Notes

- Mobile responsive; the nav collapses to a "Menu" toggle under ~860px.
- Header and footer are duplicated across pages (no templating in plain HTML).
  If you change one, update all five — or later move to a static-site generator
  (Eleventy, Astro, Jekyll) that supports shared partials.
