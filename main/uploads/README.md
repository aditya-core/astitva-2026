# ZEPHYR '26 — College Fest Website

Mobile-first, static, zero dependencies. No build step, no backend — upload the folder and it's live.

## Files

```
fest-site/
├── index.html        Home: hero + countdown · about · register-by-category · gallery
├── events.html       21 events, filterable, each with its own Register button
├── schedule.html     3-day timeline with day tabs
├── register.html     Category buttons → Google Forms, plus the Contact Us block
├── assets/
│   ├── css/style.css   Design system (mobile base → desktop at 900px)
│   ├── js/main.js      All interactions + the Google Form link config
│   └── img/
│       ├── hero.jpg, about.jpg
│       └── gallery/    10 photos (add as many as you want)
└── README.md
```

## Run it locally

```bash
cd fest-site
python3 -m http.server 3000
# open http://localhost:3000
```

## Deploy free

- **GitHub Pages** — push the folder → Settings → Pages → deploy from `main` / root.
- **Netlify / Vercel** — drag and drop the folder. No build command.

---

## 1. Connect your Google Forms

Every Register button is a plain link. There are **two ways** to set them:

**A. One form for everything** (simplest) — open `assets/js/main.js` and edit the
config at the very top:

```js
forms: {
  default: "https://docs.google.com/forms/d/e/YOUR-FORM-ID/viewform",
}
```

**B. A different form per category** — add a line using the same word as the
`data-form` attribute in the HTML:

```js
forms: {
  default: "https://docs.google.com/forms/d/e/FALLBACK/viewform",
  sports:  "https://docs.google.com/forms/d/e/SPORTS/viewform",
  debate:  "https://docs.google.com/forms/d/e/DEBATE/viewform",
  esports: "https://docs.google.com/forms/d/e/ESPORTS/viewform",
}
```

Keys in use: `cultural`, `sports`, `debate`, `esports`, `technical`, `literary`, `art`, `general`.

Every such button opens in a **new tab**. If JS is off, the link in the HTML still
works — just replace `YOUR-FORM-ID` there too.

## 2. Add gallery photos

Drop your photos in `assets/img/gallery/` and copy any block in `index.html`:

```html
<figure class="gitem reveal" data-cap="Battle of Bands">
  <img src="assets/img/gallery/g10.jpg" alt="Band on stage" loading="lazy" />
  <figcaption class="gitem__cap">Battle of Bands</figcaption>
</figure>
```

- The grid is **2 columns on phones, 4 on desktop** and auto-flows — 30+ photos are fine.
- Add the class `is-hidden` to keep a photo behind the **View all photos** button
  (the first 6 show immediately, the rest load only when tapped — keeps the page fast).
- Add `gitem--wide` to any photo you want as a big feature tile.
- Tapping a photo opens a full-screen viewer with swipe / arrow navigation.

## 3. Other things you'll edit

| What | Where |
|---|---|
| Fest dates + countdown target | `assets/js/main.js` → `CONFIG.festStart` / `festEnd` |
| College name, email, phone, address | Footer of each page + `register.html` contact section |
| Social links | Footer of each page (`href="#"`) |
| Map | `register.html` → the `<iframe src="https://maps.google.com/maps?q=...">` — replace the `q=` text with your campus |
| Colours | `assets/css/style.css` → `:root` at the top |
| Events | `events.html` — copy an `.ecard` block, set `data-cat` to match a filter button |
| Schedule | `schedule.html` — copy a `.tl-item` into the right `.day-panel` |
| Menu items | Header + drawer markup in each page (they're identical) |

---

## Mobile-first notes

- Base CSS is written for a phone; `min-width` breakpoints at **600 / 900 / 1200px** add wider layouts.
- **Hamburger** slides in a right-side drawer with a blurred backdrop; closes on backdrop tap,
  link tap, the ✕ or Escape. Body scroll locks while it's open.
- **Sticky bottom bar** (phones only) keeps *Get Registered* always one thumb away; it slides
  away when you reach the footer.
- All tap targets are **≥ 44px**, buttons ≥ 50px tall.
- Content reveals as you scroll, stat counters animate, and the countdown ticks live.
- `prefers-reduced-motion` turns the animations off automatically.

## Tested

Headless Chrome at **390 × 844** (phone) and **1440 × 900** (desktop): no console or runtime
errors, no horizontal overflow, drawer/filters/tabs/gallery-lightbox/form-links all working,
every image loading.
