# Serena Chua Travel — website

A five-page site for an independent (contractor) leisure travel agent, in the
**"Departures"** design direction: navy / bone / amber, signage-style display
type, and a travel-document motif. Each page has its own layout so no two read
the same. Plain static HTML + one CSS file — no build step — hosts on **GitHub
Pages** as-is.

> Still a design pass, not final content: a few facts are marked `[TBC]` and the
> payment / form links are placeholders. Work through the checklist below to go live.

## Pages — one job each, one layout each

| File | Page | Layout metaphor | Job |
|------|------|-----------------|-----|
| `index.html` | Home | Departures board | "Where to?" — wordmark, 3 destinations, one CTA |
| `about.html` | About | Passport spread | Who Serena is — portrait, short intro, fact "stamps" |
| `services.html` | Services | Atlas index | What she arranges — a typographic list |
| `packages.html` | Trips | Boarding-pass tickets | Book now — wide ticket rows with price + book |
| `contact.html` | Enquire | Booking slip | Start a trip — one focused form |
| `styles.css` | — | — | One system: tokens in `:root`, page-scoped `body.page-*` blocks |

Type is loaded from Google Fonts (Saira Condensed · Inter · JetBrains Mono).
The nav is a slim shared bar; the footer is a single line — deliberately no
repeated content blocks across pages.

## Preview locally

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Deploy to GitHub Pages

Already deployed at **https://achakerian.github.io/serenachua-travel/**. Any push
to `main` redeploys. (Settings → Pages → Deploy from branch `main` / root.)
Relative links mean it works under the project sub-path unchanged.

## Payments (GitHub Pages has no backend)

Static hosting can't process payments. The **Book** buttons on `packages.html`
use hosted checkout links instead:

1. In **Stripe** create a *Payment Link* (or a **PayPal** button / PayPal.me).
2. Replace each `href="#REPLACE_WITH_PAYMENT_LINK"` in `packages.html` with it.
   (The amber "LINK TBD" tag disappears automatically once the href changes.)

Clicking then sends the client to Stripe/PayPal's own secure checkout. For custom
trips, clients use the enquiry form and you send a deposit link manually.

## Enquiry form (needs a form service)

The form in `contact.html` is pre-wired for **Formspree**:

1. Create a free form at <https://formspree.io>.
2. Replace `YOUR_FORM_ID` in the `<form action="…">` with your endpoint.

Alternatives that work the same way: Netlify Forms, Getform, Web3Forms.

## Checklist before launch

- [ ] **Colours / fonts** — tune the `:root` tokens in `styles.css` if desired
- [ ] **Portrait** — real photo in `about.html` (the striped block)
- [ ] **`[TBC]` facts** — city, ATOL status, email, prices/deposits
- [ ] **Trips** — real destinations, prices, nights, inclusions in `packages.html`
- [ ] **Payment links** — Stripe/PayPal on each ticket (see above)
- [ ] **Form endpoint** — Formspree ID (see above)
- [ ] **Legal** — privacy policy link, booking terms, company details

## Notes

- Responsive to mobile; nav collapses to a "Menu" toggle under 780px.
- Reduced-motion and keyboard focus are respected.
- No templating in plain HTML, so the shared nav/footer markup lives in each
  file — edit one, edit all five. Move to a static-site generator (Eleventy,
  Astro, Jekyll) later if that becomes annoying.
