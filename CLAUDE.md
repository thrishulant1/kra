# Project context: Koramangala Realtors Association (R) website

Read this file before making any change to `index.html`.

## What this project is

This repository holds the official public website of the **Koramangala Realtors Association (R)**, also called **KRA**. Its Kannada name is ಕೋರಮಂಗಲ ರಿಯಾಲ್ಟರ್ಸ್ ಅಸೋಸಿಯೇಷನ್ (ರಿ).

KRA is a registered association (Reg. 2021-22) of real estate professionals in Koramangala, Bengaluru, Karnataka, India. Its members are brokers, builders, promoters and landlords. The tagline is "Together we grow, together we build."

The site is a **public information website**. It has:

- no membership sign-up
- no login for visitors
- no payments

Visitors use it to learn:

- who KRA is
- who leads it (the Adhyaksha/President, the committee and the trustees)
- who its members are
- what events and activities it runs
- what real estate in Koramangala is like

A digital member ID card feature may be added later. Do not build it unless asked.

**Audience:** Most visitors use **mobile phones**. Always check changes at 390px width first, then on a laptop.

## Who works on this

Thrishula coordinates the project. KRA sends content (photos, names, details) on WhatsApp. Thrishula moves it to a Google Drive folder and then into this site. Treat every name and fact below as coming from KRA's materials. Do not invent real people, phone numbers or facts. Use clearly marked placeholders instead.

## Files and hosting

```
/
├── index.html   ← the entire website (HTML + CSS + JS + images, all in one file)
└── CLAUDE.md    ← this file
```

- Hosted on **GitHub Pages** from the `main` branch, root folder. Updating means replacing `index.html` and committing.
- Later, KRA's own domain (a `.in` domain registered in KRA's name, not a person's name) will point to this site.
- Long-term plan: move to **WordPress** with a private admin login. The committee can then add events, gallery albums, committee changes, members and notices without code. The sections in this site are designed to map one-to-one onto WordPress pages and post types.

## How index.html is built

The site is a single, self-contained page with no build step, no framework and no npm.

- **CSS** sits in one `<style>` block in `<head>`. All colours are CSS variables on `:root`, with a dark-mode set under `prefers-color-scheme: dark` and `[data-theme="dark"]`.
- **JS** sits in one `<script>` block at the end of `<body>`. It is plain vanilla JavaScript.
- **Images** are embedded as **base64 data URIs** (`data:image/jpeg;base64,...`). This makes the file about 2 MB, with very long lines.
  - Do not try to read or rewrite the base64 strings.
  - When editing near them, match on the surrounding HTML or JS, not on the image data.
- **Fonts** come from Google Fonts: "Anek Latin" for English and "Anek Kannada" for Kannada, with system fallbacks. Kannada text uses the `.kn` class.

### Recommended refactor (only if asked)

The site could move images out of base64 into an `/images/` folder, e.g. `images/gallery/cricket-2026/01.jpg`, and reference them by path. This makes the file small and easy to edit. If you do this:

- Resize photos to at most 1200px on the long side.
- Compress them as JPEG at quality 70–75.
- Keep `loading="lazy"` on gallery images.

## Page sections, in order

Each section is a `<section id="...">`, and the nav links point to these ids.

| id | What it shows |
|---|---|
| `home` (on `<main>`) | Hero: association name in English and Kannada, tagline, two buttons, and two team photos (`#teams`). On mobile the photos come first and auto-rotate every 4 seconds, with dots (`#teams-dots`). On a laptop both show side by side. |
| `about` | About KRA (draft text) and four values. |
| `leadership` | Adhyaksha message (draft), the current committee of 6 with photos, trustees (placeholders), and past committees (placeholder). |
| `events` | Upcoming events (rendered from the JS `upcoming` array into `#upcoming`). Past events: KRA Cricket Tournament 2026 with its details, banner image and league fixtures. |
| `sponsors` | Sponsor posters (`#posters`, auto-rotating on mobile with dots `#posters-dots`) and sponsor cards. |
| `gallery` | Album filter buttons (`#albums`) and a photo grid (`#gallery-grid`), rendered from the JS `photos` array. Tapping a photo opens a lightbox (`#lb`). |
| `members` | Searchable member directory (`#msearch`, `#mrows`), rendered from the JS `members` array. |
| `documents` | List of documents, all marked "Coming soon". |
| `koramangala` | "Real estate in Koramangala": explainer for the public (draft). |
| `contact` | Address, phone and email. All are placeholders, marked with the `.todo` class in red. |
| `footer` | Social icons: Facebook, Instagram, X, YouTube, WhatsApp and LinkedIn, all with `href="#"` until KRA gives the links. Copyright line and Kannada tagline. |

There is **no membership form**. It was removed on purpose, so do not add it back.

## Editable data (JS arrays in the `<script>` block)

The most common updates happen here.

**Upcoming events.** Each event is one object. When the list is empty, keep one "To be announced" entry.

```js
const upcoming=[
 {date:"To be announced",title:"Next KRA event",place:"Koramangala, Bengaluru",note:"..."}
];
```

**Gallery photos.** Each photo has `cap` (caption), `ev` (album name) and `src` (image). Album buttons are built automatically from the distinct `ev` values, plus "All".

```js
const photos=[ {cap:"Prize distribution with the committee", ev:"Cricket Tournament 2026", src:"data:image/jpeg;base64,..."}, ... ];
```

To add a new event album, add photos with a new `ev` name, e.g. `ev:"Ganesha Festival 2026"`. Do not add near-duplicate photos (the same group in the same pose). Pick the one where the most people are clearly visible.

**Members.** Rows with `s:1` are dummy rows and show a "Sample" badge. Remove them when the real list arrives.

```js
const members=[ {n:"Name", f:"Firm", a:"Block/area", y:"Member since", s:1}, ... ];
```

**Carousels.** `carousel(box, dotsEl, delayMs)` in the script makes any horizontal strip auto-rotate with dots, but only when it overflows (on mobile). Rotation pauses for 8 seconds after a touch, stops when the strip is off-screen, and is disabled for `prefers-reduced-motion`. Tapping any image in `#teams`, `#posters` or `#gallery-grid` opens the lightbox. To add a new carousel, give it slides as direct children and call `carousel(...)`.

**Static HTML.** These parts are plain HTML, not data arrays. Edit them directly:

- the committee
- trustees
- sponsors
- fixtures
- documents
- contact details

## Confirmed content (from KRA's materials)

### Current committee (office bearers)

These match the order on the official tournament banner.

| Role | Kannada | Name |
|---|---|---|
| President (Adhyaksha) | ಅಧ್ಯಕ್ಷರು | Karthik |
| Vice President | ಉಪಾಧ್ಯಕ್ಷರು | Nagaraj N G V |
| General Secretary | ಪ್ರಧಾನ ಕಾರ್ಯದರ್ಶಿ | Preetham Gowda C L |
| Secretary | ಕಾರ್ಯದರ್ಶಿ | Swamy Sajjan |
| Joint Secretary | ಜಂಟಿ ಕಾರ್ಯದರ್ಶಿ | Shivakumar Gowda |
| Treasurer | ಖಜಾಂಚಿ | Sathish Reddy |

The English spellings were transliterated from Kannada and still need KRA's confirmation.

### KRA Cricket Tournament 2026

- **Date and time:** Monday, 31 August 2026, 7:00 AM onwards.
- **Venue:** St. John's Sports Ground, Hosur Road, Bengaluru (Ground C).
- **Format:** 6 teams, one day: league matches (6 overs each), semi-finals and final.
- **Chief guest:** Minister for Forest and Environment, Government of Karnataka. The name is not given in the materials, so do not add one.
- **League fixtures:**

| Time | Match |
|---|---|
| 07:00 | Team B Kusha vs Team A |
| 07:55 | Team C vs Team E |
| 08:50 | Team C vs Team F |
| 09:45 | Team E vs Team D |
| 10:40 | Team B Kusha vs Team F |
| 11:35 | Team A vs Team D |

- **Not yet known:** semi-final and final results.
- **Teams in photos:** two teams appear, an orange-and-blue jersey team and a light-blue jersey team, both with trophies.

### Sponsors

| Sponsor | Role and details |
|---|---|
| Sri C. Venkatesh | Food sponsor (breakfast and lunch). Sri Vrushabhadri Enterprises, Realtors and Promoters, Koramangala. Founder and Director, Bettada Shri Kaalabhairaveshwara Temple, Therubeedhi. |
| Antony Raj | Sports ground sponsor. Social worker and entrepreneur. |
| Manjunath G. | Sports ground sponsor. Former BBMP Corporator, S.G. Palya Ward No. 152. |
| S.S. Enterprises | G.N. Shiva Shankar, Managing Director. #293, 4th Cross, 7th Block, Koramangala, Bengaluru 560095. |
| Yogendra Builders | Building construction. |
| Yashas Real Estate | Nagaraj, Koramangala. |
| Vasudev Reddy | Sponsor. |

## Still placeholder / waiting for KRA

- KRA's own About text and the President's message (current text is a draft written for them).
- Trustee and founding member names, and past committees from 2021-22 onward.
- Real members list (remove the `s:1` sample rows).
- Office address, phone/WhatsApp and email (email is a placeholder until the domain exists).
- Social media links (footer `href="#"`).
- Documents (PDFs): registration certificate, bye-laws, circulars.
- Tournament results.
- Upcoming event details.
- A clean, high-resolution KRA logo. The current one was cropped from a photo of the banner.

## Design rules (keep the site consistent)

**Palette** (CSS variables). Green and marigold come from Bengaluru's tree canopy and the Karnataka flag colours.

| Variable | Colour | Use |
|---|---|---|
| `--green` | `#1E4A3B` | primary |
| `--marigold` | `#E3A21A` | accent |
| `--red` | `#A12A1F` | sparing use only (dates, "to-do" notes) |
| `--paper` | `#F4F6F2` | page background |
| `--surface` | `#FFFFFF` | alternate sections (`.alt`) |
| `--ink` | `#17201C` | text |

Always use these variables. Never hard-code new colours.

**Bilingual headings.** Each section heading is English `<h2>` plus a Kannada `<span class="kn">` beside it. Keep this pattern for new sections.

**Text style:**

- Sentence case everywhere.
- No ALL-CAPS labels.
- No "→" arrows on buttons.
- Plain, simple English. Visitors include non-technical local people.

**Layout:** mobile-first and responsive.

- Wide tables and strips scroll inside their own container (`overflow-x:auto`). The page itself must never scroll sideways.
- Safe-area padding is set on `:root` for phones with notches. Keep the `viewport-fit=cover` meta tag.

**Accessibility:**

- Images need meaningful `alt` text (empty `alt` only for decorative duplicates).
- Buttons are real `<button>` elements.
- Keep the visible focus outline.
- `prefers-reduced-motion` is respected.

**Data and storage:** No external images or tracking scripts. Do not add `localStorage` features unless asked.

## Before committing a change

1. Open `index.html` in a browser at phone width (390px) and laptop width (1280px).
2. Check that the browser console has no JS errors.
3. Check that nav links jump to the right sections, that the mobile "Menu" button opens and closes, that the gallery album buttons filter correctly, that the lightbox opens and closes (including the Esc key), and that the member search filters rows.
4. Keep the file name `index.html`. GitHub Pages needs it.
