# Serenely Yours Vintage Redesign — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the site's five-theme gradient identity with a single cohesive vintage love-letter identity (oxblood/mauve, Playfair + Pinyon Script + Special Elite + EB Garamond), rebuilding all five pages to match the client's 8-page mockup.

**Architecture:** Plain static HTML + one shared `styles.css`, no build step, no framework. Every colour is a single-palette CSS variable (no per-theme blocks, no theme switching). Nav/footer markup is duplicated per page (no templating). Decorative elements (torn paper, wax seals, scroll frame, postmark, airmail border, WANTED stamp) are pure CSS/SVG — no external image assets.

**Tech Stack:** HTML5, CSS3 (custom properties, `clip-path`, `repeating-linear-gradient`, scroll-snap, inline SVG), Google Fonts. Verified by rendering with headless Google Chrome.

## Global Constraints

- **No build step / no framework / no dependencies.** Static files only; must work opened directly and under GitHub Pages sub-path (relative links only).
- **No external image assets** for decoration — CSS/SVG only (inline SVG or `data:` URIs).
- **Single identity** — no theme picker, no `[data-theme]` blocks, no no-flash theme `<script>`. Remove all of it.
- **Palette tokens (verbatim):** `--wine:#5a1626` · `--wine-deep:#4a1120` · `--wine-line:#743349` · `--mauve:#a85a7a` · `--rose-faded:#b3768d` · `--cream:#f2e8e4` · `--cream-dim:rgba(242,232,228,.72)` · `--paper:#efe3dd` · `--ink:#20141a` · `--stamp-red:#a23a2e`.
- **Fonts (verbatim link):** replace the whole Google-Fonts `<link>` on every page with:
  `https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400;1,700&family=Pinyon+Script&family=Special+Elite&family=EB+Garamond:ital,wght@0,400;0,500;0,600;1,400&display=swap`
- **Type roles:** Pinyon Script = "Serenely Yours," signature only · Playfair Display = caps headlines + flowing italic lines · Special Elite = all typewriter text · EB Garamond = prose.
- **Nav (identical every page):** left links `Home · About · Gist · Sample`; centred script brand `Serenely Yours,`; right outlined rectangular `Start` button → `contact.html`. Active page marked `aria-current="page"`. Collapses under 780px via the existing `.nav__toggle` "Menu" button.
- **Accessibility:** keep keyboard focus visible; respect `prefers-reduced-motion` (no transforms/animation when set); every `<img>`/decorative block has appropriate `alt`/`aria`.
- **Content markers:** example/placeholder content keeps a visible `[TBC]` marker (testimonials, portrait, contact email), matching the existing convention.
- **Mockup source of truth:** the rendered mockup pages are at
  `/private/tmp/claude-501/-Users-aaronchakerian-codebase-serenachua-travel/0d9465ca-a51a-49cb-90ef-da4d6d2a1dfd/scratchpad/ref/page-0N.png`
  (01–02 Home, 03–04 About, 05 Gist, 06 Sample, 07–08 Start). Match layout, colour, and type to these.
- **Verify with headless Chrome.** Reusable pattern (used in every task's verify step):
  ```bash
  CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
  ROOT="/Users/aaronchakerian/codebase/serenachua-travel"
  SHOT="$ROOT/../shots"; mkdir -p "$SHOT"
  "$CHROME" --headless --hide-scrollbars --force-device-scale-factor=1 \
    --window-size=1440,2200 --screenshot="$SHOT/<page>.png" \
    "file://$ROOT/<page>.html"
  ```
  Then Read `$SHOT/<page>.png` and compare against the mockup PNG. (Fonts load from Google Fonts over the network; if offline, layout still verifies with fallback faces.)
- **Commits:** conventional-commit messages; do **not** add `Co-Authored-By` or any co-author trailer (repo owner is sole author). Work stays on branch `redesign/serenely-yours`.

---

## File Structure

- `styles.css` — single shared stylesheet. Rewritten: remove five theme blocks + gradient component CSS; add the vintage system. Organised top→bottom: (1) tokens, (2) base/reset + type helpers, (3) nav + footer + buttons, (4) home, (5) about, (6) gist, (7) sample, (8) start, (9) responsive + reduced-motion.
- `index.html` — Home: hero + Dear Passenger letter + Love Letters carousel + itinerary teaser.
- `about.html` — About: headline + WANTED poster + prose + sign-off + wax-seal stats.
- `how-it-works.html` — Gist: heading + italic line + three dashed typewriter cards.
- `sample.html` — Sample: "Previously on…" + scroll frame + restyled Strict/Options toggle over the PDF viewer.
- `contact.html` — Start: airmail brief form + wake-up spinner + interest pills + placeholder dropzones.
- `README.md` — rewritten to document the single Serenely Yours identity; theme-system docs removed.
- `pdf/*` — untouched.

Shared contract produced by Task 1 and consumed by Tasks 2–6 (exact names):
- Tokens: the palette variables above, plus `--script:"Pinyon Script",cursive` · `--display:"Playfair Display",serif` · `--type:"Special Elite",monospace` · `--body:"EB Garamond",Georgia,serif` · `--maxw:1180px` · `--radius:4px`.
- Layout: `.wrap` (max-width container, 28px side padding).
- Nav: `.nav`, `.nav__toggle`, `.nav__links`, `.nav__brand`, `.nav__cta`.
- Footer: `.foot`, `.foot__mark`.
- Type helpers: `.script` (Pinyon), `.display` (Playfair caps), `.italic-display` (Playfair italic), `.type` (Special Elite), `.code` (Special Elite caption, letter-spaced uppercase), `.tbc` (muted marker).
- Buttons: `.btn`, `.btn--outline` (rectangular outlined, cream on wine), `.btn--solid` (mauve fill).

---

## Task 1: Foundation — tokens, base, shared nav/footer, every page's `<head>`

**Files:**
- Modify: `styles.css` (replace lines 1–~200: theme blocks + base + buttons + nav/footer with the new system; leave lower page-specific CSS to later tasks — delete the old page CSS as you reach it)
- Modify: `index.html`, `about.html`, `how-it-works.html`, `sample.html`, `contact.html` (`<head>` + `<nav>` + `<footer>` on each)

**Interfaces:**
- Consumes: nothing (first task).
- Produces: the full shared contract listed in File Structure (tokens, `.wrap`, `.nav*`, `.foot*`, type helpers, `.btn*`). All later tasks rely on these exact class/token names.

- [ ] **Step 1: Replace the top of `styles.css` with the vintage foundation**

Delete the five `[data-theme]` blocks and the `:root` gradient tokens. Replace with:

```css
/* ============================================================
   Serenely Yours — bespoke itinerary planning
   Vintage love-letter identity: oxblood ground, mauve nav band,
   Playfair headlines, Pinyon signature, Special Elite typewriter,
   EB Garamond prose. Single palette — no themes.
   ============================================================ */
:root{
  --wine:#5a1626; --wine-deep:#4a1120; --wine-line:#743349;
  --mauve:#a85a7a; --rose-faded:#b3768d;
  --cream:#f2e8e4; --cream-dim:rgba(242,232,228,.72);
  --paper:#efe3dd; --ink:#20141a; --stamp-red:#a23a2e;
  --script:"Pinyon Script", cursive;
  --display:"Playfair Display", Georgia, serif;
  --type:"Special Elite", "Courier New", monospace;
  --body:"EB Garamond", Georgia, serif;
  --maxw:1180px; --radius:4px;
}
*{ box-sizing:border-box; }
html{ scroll-behavior:smooth; }
body{
  margin:0; font-family:var(--body); font-size:19px; line-height:1.55;
  color:var(--cream); background:var(--wine);
  -webkit-font-smoothing:antialiased; text-rendering:optimizeLegibility;
}
img{ max-width:100%; display:block; }
a{ color:inherit; }
.wrap{ max-width:var(--maxw); margin:0 auto; padding:0 28px; }

/* type helpers */
.script{ font-family:var(--script); font-weight:400; }
.display{ font-family:var(--display); font-weight:700; text-transform:uppercase; line-height:1.02; letter-spacing:.01em; }
.italic-display{ font-family:var(--display); font-style:italic; font-weight:400; line-height:1.05; }
.type{ font-family:var(--type); font-weight:400; }
.code{ font-family:var(--type); font-size:12px; letter-spacing:.22em; text-transform:uppercase; color:var(--cream-dim); }
.tbc{ font-family:var(--type); font-size:.8em; letter-spacing:.05em; opacity:.5; }

/* buttons */
.btn{ display:inline-flex; align-items:center; gap:.6em; font-family:var(--type);
  font-size:13px; letter-spacing:.18em; text-transform:uppercase; text-decoration:none;
  padding:13px 26px; border-radius:var(--radius); cursor:pointer; transition:background .18s, color .18s; }
.btn--outline{ color:var(--cream); border:1.5px solid var(--cream); background:none; }
.btn--outline:hover{ background:var(--cream); color:var(--wine); }
.btn--solid{ color:var(--cream); background:var(--mauve); border:1.5px solid var(--mauve); }
.btn--solid:hover{ filter:brightness(1.08); }

/* nav — mauve band, centred script brand */
.nav{ position:relative; display:flex; align-items:center; justify-content:space-between;
  background:var(--mauve); color:var(--cream); padding:18px 32px; }
.nav__links{ display:flex; gap:34px; }
.nav__links a{ font-family:var(--type); font-size:14px; letter-spacing:.14em; text-transform:uppercase; text-decoration:none; opacity:.85; }
.nav__links a:hover, .nav__links a[aria-current="page"]{ opacity:1; font-weight:400; color:#fff; }
.nav__brand{ position:absolute; left:50%; top:50%; transform:translate(-50%,-50%);
  font-family:var(--script); font-size:40px; color:var(--cream); text-decoration:none; white-space:nowrap; }
.nav__cta{ font-family:var(--type); font-size:13px; letter-spacing:.16em; text-transform:uppercase;
  text-decoration:none; color:var(--cream); border:1.5px solid var(--cream); padding:12px 30px; }
.nav__cta:hover{ background:var(--cream); color:var(--wine); }
.nav__toggle{ display:none; }

/* footer */
.foot{ display:flex; align-items:center; justify-content:space-between;
  padding:26px 32px; border-top:1px solid var(--wine-line); color:var(--cream-dim); font-family:var(--type); font-size:13px; letter-spacing:.08em; }
.foot__mark{ font-family:var(--script); font-size:30px; color:var(--cream); letter-spacing:0; text-transform:none; }

/* responsive nav */
@media (max-width:780px){
  .nav{ flex-wrap:wrap; }
  .nav__brand{ position:static; transform:none; order:-1; width:100%; text-align:center; margin-bottom:10px; }
  .nav__toggle{ display:inline-block; font-family:var(--type); font-size:13px; letter-spacing:.14em;
    text-transform:uppercase; background:none; border:1px solid var(--cream); color:var(--cream); padding:8px 16px; border-radius:var(--radius); }
  .nav__links{ display:none; flex-direction:column; gap:14px; width:100%; margin-top:14px; }
  .nav.open .nav__links{ display:flex; }
}
@media (prefers-reduced-motion:reduce){
  html{ scroll-behavior:auto; }
  *{ transition:none !important; animation:none !important; }
}
```

Delete every remaining `[data-theme]` selector and gradient-specific rule (`.grad-text`, `--grad`, `.btn--amber`, `.tcard*`, `.themes*`, `.stickers*` etc.) as you encounter them — those components are being removed. Leave the old page-body CSS below for now; later tasks replace each page's section.

- [ ] **Step 2: Update every page `<head>`**

In all five HTML files: (a) **delete** the no-flash theme `<script>` line (the `(function(){...ok=['sorbet'...]})()` IIFE); (b) **replace** the Google-Fonts `<link href=...>` with the verbatim fonts link from Global Constraints; keep the two `preconnect` links. Titles stay page-appropriate.

- [ ] **Step 3: Replace `<nav>` on every page**

Use this markup (set `aria-current="page"` on the current page's link; on `index.html` no link is current, brand is home):

```html
<nav class="nav">
  <button class="nav__toggle" aria-label="Menu" onclick="this.closest('.nav').classList.toggle('open')">Menu</button>
  <div class="nav__links">
    <a href="index.html">Home</a>
    <a href="about.html">About</a>
    <a href="how-it-works.html">Gist</a>
    <a href="sample.html">Sample</a>
  </div>
  <a class="nav__brand" href="index.html">Serenely Yours,</a>
  <a class="nav__cta" href="contact.html">Start</a>
</nav>
```

- [ ] **Step 4: Replace `<footer>` on every page**

```html
<footer class="foot">
  <span class="foot__mark script">Serenely Yours,</span>
  <span>Bespoke itinerary planning · <a href="mailto:hello@example.com">hello@serenachua.travel</a> <span class="tbc">[TBC]</span></span>
</footer>
```
Also set `<body class="page-home">` / `page-about` / `page-gist` / `page-sample` / `page-start` respectively (drop the old `dark`/`light` classes).

- [ ] **Step 5: Verify nav + footer + fonts render on every page**

Run the headless-Chrome pattern for `index.html` and `contact.html`. Read both PNGs.
Expected: mauve nav band across the top with cream uppercase links left, script "Serenely Yours," centred, outlined "START" button right; wine page background; script footer at the bottom. Page bodies below may still look unstyled/old — that's fine at this task.

- [ ] **Step 6: Commit**

```bash
git add styles.css index.html about.html how-it-works.html sample.html contact.html
git commit -m "feat: vintage foundation — tokens, fonts, shared nav/footer; remove theme system"
```

---

## Task 2: Home page — hero, Dear Passenger letter, Love Letters carousel, teaser

**Files:**
- Modify: `index.html` (replace `<main>` content)
- Modify: `styles.css` (add `.page-home` section; delete old `.home*`, `.stickers*`, `.quotes*`, `.themes*` rules)
- Verify against: mockup `page-01.png`, `page-02.png`

**Interfaces:**
- Consumes: Task 1 tokens + `.wrap` + type helpers + `.btn--outline`.
- Produces: `.letter` component (reused visually nowhere else, but the torn-paper technique is referenced by Task 4's cards conceptually).

- [ ] **Step 1: Replace `<main>` in `index.html`**

```html
<main>
  <section class="hero wrap">
    <div class="hero__copy">
      <h1 class="hero__title display">Your time<br>is precious.</h1>
      <p class="hero__tag italic-display">waste mine instead.</p>
      <p class="hero__sub">You bring the city, the hotel and the dates. I plan what you actually do each day — tuned to when you wake, what you love, and your budget. No fixed packages; just your days, handled.</p>
      <a class="btn btn--outline" href="contact.html">Start your brief →</a>
    </div>
    <aside class="letter" aria-label="A note from Serena">
      <div class="letter__body type">
        <p class="letter__hi">Dear Passenger,</p>
        <p>Do nothing.</p>
        <p>Well… except pack your bags and prepare for the adventure of a lifetime.</p>
        <p>The rest? Leave it to me.</p>
      </div>
      <p class="letter__sign"><span class="script">Serenely Yours,</span><br><span class="type">Serena</span></p>
    </aside>
  </section>

  <section class="loveletters wrap" aria-label="Love letters">
    <h2 class="loveletters__head display">Love Letters</h2>
    <p class="code">Example — real notes land here as trips wrap <span class="tbc">[TBC]</span></p>
    <div class="carousel" data-carousel>
      <button class="carousel__nav carousel__nav--prev" aria-label="Previous">‹</button>
      <div class="carousel__track">
        <figure class="ll"><blockquote class="type">All sorted before we'd even landed. We just turned up and enjoyed it.</blockquote><figcaption class="type">Serenely,<br>Adam &amp; Maya</figcaption></figure>
        <figure class="ll"><blockquote class="type">No stress, no spreadsheets, no wasted days. Easily the best trip we've taken.</blockquote><figcaption class="type">Serenely,<br>Priya</figcaption></figure>
        <figure class="ll"><blockquote class="type">Every day was easy, fun, and exactly us — right down to where we ate.</blockquote><figcaption class="type">Serenely,<br>The Alvarez family</figcaption></figure>
      </div>
      <button class="carousel__nav carousel__nav--next" aria-label="Next">›</button>
      <div class="carousel__dots" aria-hidden="true"><i class="on"></i><i></i><i></i></div>
    </div>
  </section>

  <section class="teaser wrap">
    <a class="teaser__frame" href="sample.html" aria-label="See a sample itinerary">
      <span class="teaser__label code">Previously on…</span>
      <span class="teaser__doc"></span>
      <span class="teaser__cta type">See a sample day →</span>
    </a>
  </section>
</main>
```

- [ ] **Step 2: Add the home CSS to `styles.css`**

Concrete decorative code for the torn letter + carousel (tune spacing to the mockup):

```css
.hero{ display:grid; grid-template-columns:1fr 1fr; gap:40px; align-items:center; padding:7vh 28px 9vh; }
.hero__title{ font-size:clamp(48px,7vw,104px); color:var(--rose-faded); margin:0; }
.hero__tag{ font-size:clamp(30px,4.5vw,60px); color:var(--cream); margin:.1em 0 .6em; }
.hero__sub{ max-width:34ch; color:var(--cream-dim); margin:0 0 1.6em; }

/* torn lined-paper letter */
.letter{ position:relative; background:var(--paper); color:var(--ink);
  padding:52px 46px 60px; transform:rotate(2deg); box-shadow:0 24px 50px -18px rgba(0,0,0,.55);
  background-image:repeating-linear-gradient(180deg, transparent 0 33px, rgba(60,40,50,.16) 33px 34px);
  clip-path:polygon(0 2%,8% 0,22% 3%,38% 0,54% 3%,70% 0,86% 2%,100% 0,99% 15%,100% 34%,98% 52%,100% 70%,99% 88%,100% 100%,80% 98%,62% 100%,44% 98%,26% 100%,10% 99%,0 100%,1% 76%,0 54%,2% 32%,0 14%); }
.letter__body p{ margin:0 0 26px; font-size:22px; }
.letter__hi{ font-size:30px !important; transform:rotate(-1.5deg); }
.letter__sign{ margin-top:28px; }
.letter__sign .script{ font-size:38px; }

/* love letters carousel */
.loveletters{ padding:6vh 28px; text-align:center; }
.loveletters__head{ font-size:clamp(30px,4vw,52px); color:var(--cream); margin:0 0 6px; }
.carousel{ position:relative; margin-top:40px; }
.carousel__track{ display:flex; overflow-x:auto; scroll-snap-type:x mandatory; gap:0; scrollbar-width:none; }
.carousel__track::-webkit-scrollbar{ display:none; }
.ll{ flex:0 0 100%; scroll-snap-align:center; margin:0; padding:0 8%; }
.ll blockquote{ font-size:clamp(18px,2.2vw,26px); text-transform:uppercase; letter-spacing:.04em; color:var(--cream); margin:0 0 28px; }
.ll figcaption{ color:var(--cream-dim); letter-spacing:.1em; }
.carousel__nav{ position:absolute; top:36%; background:none; border:none; color:var(--cream); font-size:40px; cursor:pointer; line-height:1; }
.carousel__nav--prev{ left:0; } .carousel__nav--next{ right:0; }
.carousel__dots{ display:flex; gap:10px; justify-content:center; margin-top:26px; }
.carousel__dots i{ width:9px; height:9px; border-radius:50%; background:var(--wine-line); }
.carousel__dots i.on{ background:var(--cream); }

/* itinerary teaser */
.teaser{ padding:2vh 28px 9vh; text-align:center; }
.teaser__frame{ display:inline-flex; flex-direction:column; align-items:center; gap:14px; padding:34px 48px;
  border:2px solid var(--wine-line); border-radius:var(--radius); text-decoration:none; }
.teaser__label{ display:block; }
.teaser__doc{ width:160px; height:200px; background:var(--paper); box-shadow:0 12px 28px -14px rgba(0,0,0,.6);
  background-image:repeating-linear-gradient(180deg,transparent 0 15px,rgba(60,40,50,.12) 15px 16px); }
.teaser__cta{ color:var(--cream); letter-spacing:.1em; }
@media (max-width:780px){ .hero{ grid-template-columns:1fr; } .letter{ transform:none; } }
```

- [ ] **Step 3: Add the carousel script at the bottom of `index.html`**

```html
<script>
(function(){
  var c=document.querySelector('[data-carousel]'); if(!c) return;
  var track=c.querySelector('.carousel__track'), dots=c.querySelectorAll('.carousel__dots i');
  var cards=c.querySelectorAll('.ll'), i=0;
  function go(n){ i=(n+cards.length)%cards.length; cards[i].scrollIntoView({behavior:'smooth',inline:'center',block:'nearest'});
    dots.forEach(function(d,k){ d.classList.toggle('on',k===i); }); }
  c.querySelector('.carousel__nav--prev').onclick=function(){ go(i-1); };
  c.querySelector('.carousel__nav--next').onclick=function(){ go(i+1); };
})();
</script>
```

- [ ] **Step 4: Verify against the mockup**

Run the headless-Chrome pattern for `index.html`; Read `$SHOT/index.png`; compare to `page-01.png` + `page-02.png`.
Expected: oxblood hero; "YOUR TIME IS PRECIOUS." in faded rose Playfair caps; "waste mine instead." cream italic; torn cream letter tilted right with typewriter text signed "Serenely Yours, / Serena"; below, centred "LOVE LETTERS" with one typewriter testimonial + chevrons + three dots; a framed teaser linking to Sample.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css
git commit -m "feat: home — hero, Dear Passenger letter, Love Letters carousel, teaser"
```

---

## Task 3: About page — headline, WANTED poster, prose, sign-off, wax-seal stats

**Files:**
- Modify: `about.html` (replace `<main>`)
- Modify: `styles.css` (add `.page-about` section; delete old `.passport*`, `.stamps*` rules)
- Verify against: `page-03.png`, `page-04.png`

**Interfaces:**
- Consumes: Task 1 contract.
- Produces: `.seal` component (self-contained).

- [ ] **Step 1: Replace `<main>` in `about.html`**

```html
<main class="wrap about">
  <h1 class="about__head display">I turn "what should we do?"<br>into "that was the best day ever."</h1>

  <figure class="wanted">
    <div class="wanted__photo" role="img" aria-label="Portrait of Serena [TBC]"></div>
    <span class="wanted__stamp type">Wanted</span>
    <figcaption class="wanted__cap type">One unsuspecting traveler<br>with excellent taste.</figcaption>
  </figure>

  <p class="about__prose">I simply refuse to let a perfectly good vacation be wasted on mediocre planning. I adore a hidden gem, a well-timed coffee stop, and an itinerary with just enough room for a happy little detour.</p>

  <p class="about__sign">
    <s class="type">Yours sincerely,</s><br>
    <span class="type about__name">Serena</span>
    <span class="script about__signmark">Serenely Yours,</span>
  </p>

  <section class="seals" aria-label="What you get">
    <span class="seal"><b class="type">48H</b><small class="type">Draft turnaround</small></span>
    <span class="seal"><b class="type">$0</b><small class="type">Share the vision</small></span>
    <span class="seal"><b class="type">2×</b><small class="type">Rounds of tweaks</small></span>
    <span class="seal"><b class="type">1:1</b><small class="type">My personal touch</small></span>
  </section>
</main>
```

- [ ] **Step 2: Add the about CSS**

```css
.about{ padding:6vh 28px 9vh; text-align:center; }
.about__head{ font-size:clamp(30px,4.4vw,58px); color:var(--rose-faded); max-width:22ch; margin:0 auto 5vh; }

/* WANTED poster */
.wanted{ display:inline-block; background:#d9c9b0; padding:16px 16px 20px; margin:0 auto;
  box-shadow:0 18px 40px -18px rgba(0,0,0,.6); position:relative; }
.wanted__photo{ width:300px; height:340px; background:#1c1c1c; filter:grayscale(1) contrast(1.05);
  /* placeholder gradient stands in for the real B&W portrait */
  background-image:radial-gradient(120% 90% at 50% 25%, #6a6a6a 0%, #2a2a2a 55%, #111 100%); }
.wanted__stamp{ position:absolute; top:60%; left:50%; transform:translate(-50%,-50%) rotate(-9deg);
  color:var(--stamp-red); font-size:56px; letter-spacing:.06em; text-transform:uppercase;
  border:5px solid var(--stamp-red); padding:2px 16px; opacity:.85; mix-blend-mode:multiply; }
.wanted__cap{ display:block; color:var(--ink); font-size:15px; letter-spacing:.04em; margin-top:14px; text-transform:uppercase; }

.about__prose{ font-size:clamp(20px,2.4vw,30px); max-width:26ch; margin:6vh auto 4vh; color:var(--cream); }
.about__sign{ position:relative; min-height:120px; }
.about__sign s{ font-size:20px; color:var(--cream-dim); }
.about__name{ font-size:22px; }
.about__signmark{ display:inline-block; font-size:44px; color:var(--cream); margin-left:24px; }

/* wax seals */
.seals{ display:flex; flex-wrap:wrap; gap:44px; justify-content:center; margin-top:7vh; }
.seal{ width:160px; height:160px; border-radius:48% 52% 47% 53%/52% 47% 53% 48%;
  display:flex; flex-direction:column; align-items:center; justify-content:center; text-align:center;
  background:radial-gradient(circle at 38% 32%, #7a2438 0%, var(--wine-deep) 62%, #360c17 100%);
  box-shadow:inset 0 0 0 6px rgba(0,0,0,.18), inset 0 6px 14px rgba(255,255,255,.06), 0 14px 26px -12px rgba(0,0,0,.6);
  color:var(--cream); }
.seal b{ font-size:34px; } .seal small{ font-size:11px; letter-spacing:.1em; text-transform:uppercase; opacity:.85; padding:0 12px; }
@media (max-width:780px){ .wanted__photo{ width:78vw; max-width:300px; } }
```

- [ ] **Step 3: Verify against the mockup**

Headless-Chrome `about.html`; compare `$SHOT/about.png` to `page-03.png` + `page-04.png`.
Expected: faded-rose Playfair headline; kraft WANTED poster with grayscale portrait block, red rotated "WANTED" stamp, typewriter caption; Garamond prose; strikethrough "Yours sincerely," + "Serena" + script "Serenely Yours,"; a row of four oxblood wax-seal discs with 48H / $0 / 2× / 1:1.

- [ ] **Step 4: Commit**

```bash
git add about.html styles.css
git commit -m "feat: about — WANTED poster, letter prose, wax-seal stats"
```

---

## Task 4: Gist page — heading, italic line, three dashed typewriter cards

**Files:**
- Modify: `how-it-works.html` (replace `<main>`; keep filename)
- Modify: `styles.css` (add `.page-gist` section; delete old `.how*`, `.flow*`, `.dials*` rules)
- Verify against: `page-05.png`

**Interfaces:** Consumes Task 1 contract. Produces nothing downstream.

- [ ] **Step 1: Replace `<main>` in `how-it-works.html`**

```html
<main class="wrap gist">
  <span class="code">How it works</span>
  <h1 class="gist__lead italic-display">You bring me the vision.<br>I orchestrate the experience.</h1>
  <div class="gist__cards">
    <article class="gcard"><h3 class="type">01. Set the scene</h3><p class="type">The city. The hotel. The dates. The budget.</p></article>
    <article class="gcard"><h3 class="type">02. I make it happen</h3><p class="type">I take your wish list and turn it into days worth remembering.</p></article>
    <article class="gcard"><h3 class="type">03. The big reveal</h3><p class="type">A perfectly planned itinerary, delivered.</p></article>
  </div>
</main>
```

- [ ] **Step 2: Add the gist CSS**

```css
.gist{ padding:6vh 28px 10vh; }
.gist__lead{ font-size:clamp(34px,5.5vw,76px); color:var(--rose-faded); margin:.2em 0 8vh; }
.gist__cards{ display:grid; grid-template-columns:repeat(3,1fr); gap:26px; }
.gcard{ border:1.5px dashed var(--wine-line); border-radius:var(--radius); padding:40px 30px; text-align:center; }
.gcard h3{ font-size:20px; text-transform:uppercase; letter-spacing:.06em; color:var(--cream); margin:0 0 22px; line-height:1.5; }
.gcard p{ font-size:15px; letter-spacing:.03em; color:var(--cream-dim); text-transform:uppercase; line-height:1.9; margin:0; }
@media (max-width:780px){ .gist__cards{ grid-template-columns:1fr; } }
```

- [ ] **Step 3: Verify against the mockup**

Headless-Chrome `how-it-works.html`; compare `$SHOT/how-it-works.png` to `page-05.png`.
Expected: small "HOW IT WORKS" caption; large faded-rose Playfair italic "You bring me the vision. / I orchestrate the experience."; three dashed-border cards with typewriter numbered titles + uppercase body.

- [ ] **Step 4: Commit**

```bash
git add how-it-works.html styles.css
git commit -m "feat: gist — italic lead + dashed typewriter step cards"
```

---

## Task 5: Sample page — "Previously on…", scroll frame, restyled Strict/Options toggle

**Files:**
- Modify: `sample.html` (replace `<main>`; **keep** the two `<iframe>` PDF viewers + the pure-CSS radio toggle mechanism)
- Modify: `styles.css` (add `.page-sample` section; delete old `.sample*`, `.seg*`, `.viewer*` rules and rebuild them vintage)
- Verify against: `page-06.png`

**Interfaces:** Consumes Task 1 contract. The Strict/Options toggle stays pure CSS (hidden radios + `:checked`), no JS.

- [ ] **Step 1: Replace `<main>` in `sample.html`**

```html
<main class="wrap sample">
  <h1 class="sample__head italic-display">Previously on…</h1>
  <p class="code">The actual PDF you'd receive — planned to the quarter-hour, or opened up with options.</p>

  <section class="scroll">
    <div class="scroll__rod scroll__rod--top" aria-hidden="true"></div>
    <input type="radio" name="fmt" id="fmt-strict" class="seg-input" checked>
    <input type="radio" name="fmt" id="fmt-options" class="seg-input">
    <div class="seg" role="tablist">
      <label for="fmt-strict">Strict · quarter-hour</label>
      <label for="fmt-options">Options · your pick</label>
    </div>
    <div class="scroll__paper">
      <div class="fmt fmt--strict">
        <iframe class="viewer__frame" src="pdf/sample-strict.pdf#toolbar=0&amp;view=FitH" title="Sample itinerary — strict (PDF)" loading="lazy"></iframe>
      </div>
      <div class="fmt fmt--options">
        <iframe class="viewer__frame" src="pdf/sample-options.pdf#toolbar=0&amp;view=FitH" title="Sample itinerary — options (PDF)" loading="lazy"></iframe>
      </div>
    </div>
    <div class="scroll__rod scroll__rod--bottom" aria-hidden="true"></div>
  </section>

  <p class="viewer__note code">Can't see it? Download <a href="pdf/sample-strict.pdf" download>strict</a> or <a href="pdf/sample-options.pdf" download>options</a>.</p>
  <div class="sample__cta"><a class="btn btn--outline" href="contact.html">Brief me on your trip →</a></div>
</main>
```

- [ ] **Step 2: Add the sample CSS (scroll frame + toggle + viewer)**

```css
.sample{ padding:6vh 28px 9vh; text-align:center; }
.sample__head{ font-size:clamp(40px,6vw,84px); color:var(--rose-faded); margin:0 0 8px; }
.scroll{ max-width:720px; margin:5vh auto 0; }
.scroll__rod{ height:26px; border:2px solid var(--wine-line); border-radius:14px; background:var(--wine-deep); position:relative; }
.scroll__rod::before, .scroll__rod::after{ content:""; position:absolute; top:50%; width:30px; height:38px; transform:translateY(-50%);
  border:2px solid var(--wine-line); border-radius:10px; background:var(--wine-deep); }
.scroll__rod::before{ left:-24px; } .scroll__rod::after{ right:-24px; }
.scroll__paper{ background:var(--cream); padding:14px; }
.seg-input{ position:absolute; opacity:0; pointer-events:none; }
.seg{ display:inline-flex; gap:10px; margin:18px 0; }
.seg label{ font-family:var(--type); font-size:12px; letter-spacing:.1em; text-transform:uppercase; color:var(--cream-dim);
  border:1px solid var(--wine-line); border-radius:999px; padding:9px 18px; cursor:pointer; }
#fmt-strict:checked ~ .seg label[for="fmt-strict"], #fmt-options:checked ~ .seg label[for="fmt-options"]{ background:var(--mauve); color:var(--cream); border-color:var(--mauve); }
.viewer__frame{ width:100%; height:560px; border:none; background:var(--cream); display:block; }
.fmt--options{ display:none; } .fmt--strict{ display:block; }
#fmt-options:checked ~ .scroll__paper .fmt--options{ display:block; }
#fmt-options:checked ~ .scroll__paper .fmt--strict{ display:none; }
.viewer__note{ margin-top:22px; } .viewer__note a{ text-decoration:underline; }
.sample__cta{ margin-top:5vh; }
```
Note the radios sit **before** `.seg` and `.scroll__paper` as siblings so the `:checked ~` selectors reach both — keep that ordering from Step 1.

- [ ] **Step 3: Verify against the mockup**

Headless-Chrome `sample.html`; compare `$SHOT/sample.png` to `page-06.png`.
Expected: faded-rose italic "Previously on…"; a scroll frame (top + bottom rods with finials) wrapping a cream panel that shows the embedded PDF; two pill toggles (Strict / Options) that swap which PDF shows; download fallback + outlined CTA. Manually confirm clicking "Options" swaps the iframe (toggle is CSS-only).

- [ ] **Step 4: Commit**

```bash
git add sample.html styles.css
git commit -m "feat: sample — Previously on scroll frame over Strict/Options PDF viewer"
```

---

## Task 6: Start page — airmail form, wake-up spinner, interest pills, placeholder dropzones

**Files:**
- Modify: `contact.html` (replace `<main>`; keep the Formspree `<form action>` placeholder)
- Modify: `styles.css` (add `.page-start` section; delete old `.slip*`, `.form*`, `.field*`, `.chip*` rules and rebuild vintage)
- Verify against: `page-07.png`, `page-08.png`

**Interfaces:** Consumes Task 1 contract. Dropzones are non-functional placeholder UI (Global Constraint #4).

- [ ] **Step 1: Replace `<main>` in `contact.html`**

```html
<main class="wrap start">
  <form class="airmail" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
    <div class="airmail__stamp" aria-hidden="true"><span class="type">Air<br>Mail</span></div>
    <div class="airmail__rule" aria-hidden="true"></div>

    <div class="linefield"><label for="name" class="type">Name:</label><input id="name" name="name" type="text" required></div>
    <div class="linefield"><label for="email" class="type">Email:</label><input id="email" name="email" type="email" required></div>
    <div class="linefield"><label for="city" class="type">City:</label><input id="city" name="city" type="text" required></div>
    <div class="linefield"><label for="travellers" class="type">No. of Travellers:</label><input id="travellers" name="travellers" type="number" min="1" value="2"></div>
    <div class="linefield"><label for="dates" class="type">Dates:</label><input id="dates" name="dates" type="text" placeholder="e.g. 12–15 Sept"></div>

    <div class="linefield linefield--wake">
      <label class="type" for="wake-h">Typical wake-up:</label>
      <span class="spinner">
        <select id="wake-h" name="wake_hour" aria-label="Hour">
          <option>05</option><option selected>06</option><option>07</option><option>08</option><option>09</option><option>10</option>
        </select>
        <select name="wake_min" aria-label="Minute"><option>00</option><option selected>15</option><option>30</option><option>45</option></select>
        <select name="wake_ampm" aria-label="AM or PM"><option selected>AM</option><option>PM</option></select>
      </span>
    </div>

    <div class="linefield linefield--chips">
      <span class="type">Interests:</span>
      <span class="pills">
        <label class="pill"><input type="checkbox" name="interests" value="City">City</label>
        <label class="pill"><input type="checkbox" name="interests" value="Country">Country</label>
        <label class="pill"><input type="checkbox" name="interests" value="Gourmet">Gourmet</label>
        <label class="pill"><input type="checkbox" name="interests" value="Arts">Arts</label>
      </span>
    </div>

    <div class="linefield"><label for="budget" class="type">Daily budget:</label><input id="budget" name="budget" type="text" placeholder="e.g. £150 pp/day"></div>
    <div class="linefield"><label for="format" class="type">Plan format:</label><input id="format" name="format" type="text" placeholder="Strict / Options / Not sure"></div>
    <div class="linefield"><label for="others" class="type">Others:</label><input id="others" name="others" type="text"></div>

    <div class="dropzones">
      <div class="dz"><span class="dz__label type">Flights:</span><div class="dz__box"><span class="type">drop pdf/png</span></div></div>
      <div class="dz"><span class="dz__label type">Accommodations:</span><div class="dz__box"><span class="type">drop pdf/png</span></div></div>
      <div class="dz"><span class="dz__label type">Others: <small>(eg., attractions)</small></span><div class="dz__box"><span class="type">drop pdf/png</span></div></div>
    </div>
    <p class="code dz__note">Uploads coming soon — email your files after your brief <span class="tbc">[TBC]</span></p>

    <div class="airmail__rule" aria-hidden="true"></div>
    <div class="start__submit"><button class="btn btn--outline" type="submit">Send brief →</button><span class="code">No fee to ask</span></div>
  </form>
</main>
```

- [ ] **Step 2: Add the start CSS (airmail letter, spinner, pills, dropzones)**

```css
.start{ padding:5vh 28px 9vh; }
.airmail{ position:relative; max-width:820px; margin:0 auto; }
.airmail__rule{ height:0; border-top:3px dashed transparent;
  background:repeating-linear-gradient(-45deg,var(--stamp-red) 0 14px,transparent 14px 28px,var(--mauve) 28px 42px,transparent 42px 56px); }
.airmail__stamp{ position:absolute; top:-8px; left:0; width:96px; height:96px; border-radius:50%;
  border:2px solid var(--wine-line); display:flex; align-items:center; justify-content:center; transform:rotate(-8deg);
  background:var(--wine-deep); }
.airmail__stamp span{ color:var(--cream-dim); font-size:14px; text-align:center; letter-spacing:.14em; text-transform:uppercase; }

.linefield{ display:flex; align-items:baseline; gap:16px; padding:14px 0; }
.linefield label, .linefield > span:first-child{ flex:0 0 210px; text-align:right; color:var(--cream); letter-spacing:.04em; }
.linefield input{ flex:1; background:none; border:none; border-bottom:1.5px solid var(--cream-dim);
  color:var(--cream); font-family:var(--type); font-size:17px; padding:4px 2px; }
.linefield input:focus{ outline:none; border-bottom-color:var(--cream); }

.spinner{ display:inline-flex; gap:10px; }
.spinner select{ background:var(--wine-deep); color:var(--cream); font-family:var(--type); font-size:16px;
  border:1px solid var(--wine-line); border-radius:var(--radius); padding:6px 10px; }

.pills{ display:flex; flex-wrap:wrap; gap:10px; }
.pill{ font-family:var(--type); font-size:13px; letter-spacing:.08em; text-transform:uppercase; color:var(--cream-dim);
  border:1px solid var(--wine-line); border-radius:999px; padding:8px 16px; cursor:pointer; }
.pill input{ position:absolute; opacity:0; width:0; }
.pill:has(input:checked){ background:var(--mauve); color:var(--cream); border-color:var(--mauve); }

.dropzones{ display:grid; grid-template-columns:repeat(3,1fr); gap:22px; margin:6vh 0 10px; }
.dz{ text-align:center; } .dz__label{ display:block; color:var(--cream); margin-bottom:12px; letter-spacing:.06em; }
.dz__label small{ opacity:.7; }
.dz__box{ border:2px dashed var(--wine-line); border-radius:10px; height:150px; display:flex; align-items:center; justify-content:center;
  color:var(--cream-dim); background:rgba(255,255,255,.02); transition:background .18s,border-color .18s; }
.dz__box:hover{ border-color:var(--mauve); background:rgba(168,90,122,.12); }
.dz__note{ text-align:center; margin-bottom:5vh; }
.start__submit{ display:flex; align-items:center; gap:20px; margin-top:26px; }
@media (max-width:780px){ .linefield{ flex-direction:column; } .linefield label,.linefield>span:first-child{ text-align:left; flex-basis:auto; } .dropzones{ grid-template-columns:1fr; } }
```

- [ ] **Step 3: Verify against the mockup**

Headless-Chrome `contact.html`; compare `$SHOT/contact.png` to `page-07.png` + `page-08.png`.
Expected: circular AIR MAIL postmark top-left; dashed red/mauve airmail rule; right-aligned typewriter labels with underlined input lines; wake-up spinner (three selects); four interest pills; three dashed "drop pdf/png" dropzones with hover state; submit button. Confirm a pill toggles filled on click (`:has` selector) and inputs are typable.

- [ ] **Step 4: Commit**

```bash
git add contact.html styles.css
git commit -m "feat: start — airmail brief form, wake-up spinner, interest pills, placeholder dropzones"
```

---

## Task 7: README + final cross-page pass

**Files:**
- Modify: `README.md` (rewrite identity/design sections)
- Modify: `styles.css` (delete any dead rules left from old components; confirm no `[data-theme]`/gradient remnants)
- Verify: all five pages at desktop + a 375px-wide mobile render; check links, focus, reduced-motion.

**Interfaces:** none produced.

- [ ] **Step 1: Rewrite `README.md`**

Update to describe the single "Serenely Yours" vintage identity: palette, the four fonts and their roles, per-page concepts, decorative-elements-are-CSS/SVG note, the placeholder-dropzones caveat, and the "regenerate PDFs" section (unchanged). **Remove** the entire "Five themes" / theme-switch documentation and the theme rows in the pages table. Keep the Formspree, run/deploy, and handoff sections (refresh the handoff list: real portrait for the WANTED poster, real Love Letters, business details, Formspree ID, optional real uploads/mood itineraries).

- [ ] **Step 2: Grep for dead theme code**

Run: `grep -nE "data-theme|--grad|tcard|sct-theme|Space Grotesk|Fraunces|Bricolage|Fredoka|Unbounded|JetBrains|Inter:wght" styles.css index.html about.html how-it-works.html sample.html contact.html`
Expected: **no matches**. Remove any that remain.

- [ ] **Step 3: Desktop + mobile verification of all five pages**

For each page run the headless pattern at `--window-size=1440,2200`, then again at `--window-size=375,1600`. Read the PNGs.
Expected: nav collapses to the "Menu" toggle at 375px with the script brand stacked/centred; no horizontal overflow; every page reads as one vintage identity. Confirm all nav links resolve (Home/About/Gist/Sample/Start) and the footer email is marked `[TBC]`.

- [ ] **Step 4: Commit**

```bash
git add README.md styles.css
git commit -m "docs: rewrite README for Serenely Yours identity; drop theme-system docs"
```

---

## Self-Review

**Spec coverage:** identity/no-themes → Task 1; palette + fonts → Task 1 (Global Constraints); nav/footer → Task 1; Home hero/letter/Love-Letters/teaser → Task 2; About headline/WANTED/prose/sign-off/seals → Task 3; Gist → Task 4; Sample "Previously on"/scroll/toggle → Task 5; Start airmail/spinner/pills/dropzones → Task 6; README + cleanup → Task 7. Decorative CSS/SVG technique specified per component. Out-of-scope items (PDFs, real content, real uploads) explicitly untouched. No gaps.

**Placeholder scan:** every code step contains real markup/CSS; verification steps state exact expected visuals against named mockup PNGs; copy text is verbatim. No TBD/TODO in the plan itself (in-page `[TBC]` are intentional content markers per the spec).

**Type/name consistency:** shared class/token names in Task 1's "Produces" (`.nav__cta`, `.btn--outline`, `.script`, `.italic-display`, `.type`, `.code`, tokens) are used identically in Tasks 2–6. Toggle ids `fmt-strict`/`fmt-options` and their `:checked ~` selectors are consistent within Task 5. Spinner/pill `name` attributes are self-consistent within Task 6.
