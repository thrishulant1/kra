# Project context: Koramangala Realtors Association (R) website

Read this file before making any change to `index.html`.

## What this project is

This repository holds the official public website of the **Koramangala Realtors Association (R)**, also called **KRA**. Its official Kannada name (from the KRA letterhead) is ಕೋರಮಂಗಲ ರಿಯಲ್ಟರ್ಸ್ ಅಸೋಸಿಯೇಷನ್ (ರಿ).

KRA is a registered association (Reg. No. DRB3/SOR/646/21-22, registered 2021-22). Its office is at #823, 21st 'A' Main Road, 8th Block, Koramangala, Bengaluru 560095. KRA's own forms sometimes use the heading "Real Estate Association", but the registered name is Koramangala Realtors Association (R) of real estate professionals in Koramangala, Bengaluru, Karnataka, India. Its members are brokers, builders, promoters and landlords. The tagline is "Together we grow, together we build."

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
├── index.html   ← the website (HTML + CSS + JS)
├── images/      ← all photos, named by section and number (see Image naming)
└── CLAUDE.md    ← this file
```

- Hosted on **GitHub Pages** from the `main` branch, root folder. Updating means replacing `index.html` and committing.
- Later, KRA's own domain (a `.in` domain registered in KRA's name, not a person's name) will point to this site.
- Long-term plan: move to **WordPress** with a private admin login. The committee can then add events, gallery albums, committee changes, members and notices without code. The sections in this site are designed to map one-to-one onto WordPress pages and post types.

## How index.html is built

The site is a single, self-contained page with no build step, no framework and no npm.

- **CSS** sits in one `<style>` block in `<head>`. All colours are CSS variables on `:root`, with a dark-mode set under `prefers-color-scheme: dark` and `[data-theme="dark"]`.
- **JS** sits in one `<script>` block at the end of `<body>`. It is plain vanilla JavaScript.
- **Images** live in `/images/` and are referenced by relative path (e.g. `images/home-01.jpg`). Every image except the first home page photo and the logo uses `loading="lazy"`. Keep new photos to at most about 1000px on the long side, JPEG quality 55–65, so the site stays fast on mobile data.
- **Fonts** come from Google Fonts: "Anek Latin" for English and "Anek Kannada" for Kannada, with system fallbacks. Kannada text uses the `.kn` class.

## Image naming

Every file in `/images/` is named `<section>-<number>[-<person>].jpg`, so it is clear where each photo is used:

| Prefix | Where it is used |
|---|---|
| `kra-logo.png` | Header logo |
| `home-01` … | Rotating home page photos |
| `committee-2026-NN-<name>` | Current committee cards (01 is the President; it is also used in the Adhyaksha message) |
| `committee-2021-NN-<name>` | First committee cards  |
| `event-meetings-2026-NN` / `event-cricket-2026-NN` / `event-election-2026-NN` | Rotating strips inside each past event |
| `event-cricket-2026-banner-01` | Tournament banner beside the event details |
| `sponsor-poster-NN` | Rotating sponsor posters |
| `gallery-<album>-NN` | Gallery photos, by album |

When adding a photo, follow the same pattern and continue the numbering. To replace a person's photo, overwrite the file with the same name.

## Page sections, in order

Each section is a `<section id="...">`, and the nav links point to these ids.

| id | What it shows |
|---|---|
| `home` (on `<main>`) | Hero: association name in English and Kannada, tagline, two buttons, and a rotating photo strip (`#teams`) of the 7 photos the client chose (Drive folder "Seo"), in this order: KRA committee with the minister, the meeting with Bengaluru City Police, the meeting at the association hall, KRA members, the minister meeting (second photo), and two tournament team photos. Cricket photos must not come first. Other event photos belong in the rotating strips inside each event under Events, and in the gallery. It shows 1 photo at a time on mobile (where photos come first) and 2 at a time on a laptop. The first move is after 2 seconds, then every 3 seconds, with dots (`#teams-dots`). |
| `about` | About KRA (draft text) and four values. |
| `leadership` | Adhyaksha message (draft), the current committee of 6 with photos, trustees (placeholders), and past committees (placeholder). |
| `events` | Upcoming events (rendered from the JS `upcoming` array into `#upcoming`). Past events, each with its own rotating photo strip (`#meet-strip`, `#cricket-strip`, `#election-strip`): Meetings and visits 2026; KRA Cricket Tournament 2026 (details, banner, fixtures); and the KRA Election and installation ceremony 2026. |
| `sponsors` | Sponsor posters (`#posters`, auto-rotating on mobile: first move after 1 second, then every 2 seconds, with dots `#posters-dots`) and sponsor cards. |
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

Current albums: "Meetings and visits 2026" (5 photos), "Cricket Tournament 2026" (17 photos) and "Election & Installation 2026" (20 photos). The client wants every tournament photo kept in the gallery, so do not trim albums. To add a new event album, add photos with a new `ev` name, e.g. `ev:"Ganesha Festival 2026"`.

**Members.** Rows with `s:1` are dummy rows and show a "Sample" badge. Remove them when the real list arrives.

```js
const members=[ {n:"Name", f:"Firm", a:"Block/area", y:"Member since", s:1}, ... ];
```

**Carousels.** `carousel(box, dotsEl, delayMs, firstDelayMs)` in the script makes any horizontal strip auto-rotate with dots, but only when it overflows (on mobile). Rotation pauses for 8 seconds after a touch, and stops when the strip is off-screen. Under `prefers-reduced-motion` it still rotates but jumps instead of sliding. Many phones have this setting on, and the client wants rotation everywhere. Tapping any image in `#teams`, `#posters` or `#gallery-grid` opens the lightbox. To add a new carousel, give it slides as direct children and call `carousel(...)`.

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
- **Chief guest:** Sri Ramalinga Reddy (ಶ್ರೀ ರಾಮಲಿಂಗಾರೆಡ್ಡಿ), **Minister for Forest, Ecology and Environment, Government of Karnataka**. His portfolio history:
  - Transport Minister until May 2026.
  - Major and Medium Irrigation from June 2026.
  - Forest, Ecology and Environment since 12 August 2026 (verified in news reports, October 2026).
  - If this changes again, update every mention on the site.
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
- **Do not label people in photos by name** unless KRA confirms who is in that photo.
- **Teams in photos:** two teams appear, an orange-and-blue jersey team and a light-blue jersey team, both with trophies.

### Meetings and visits 2026

- **Meeting with the minister:** the KRA committee met Sri Ramalinga Reddy (photos supplied by the client as the minister meeting).
- **Meeting with Bengaluru City Police:** the committee met officers (the office sign reads ಬೆಂಗಳೂರು ನಗರ ಪೊಲೀಸ್).
- **Meeting at the association hall:** a meeting with felicitation was held there.
- **Missing details:** dates are not yet known.

### First committee (23 March 2021)

| Post | Name | Status |
|---|---|---|
| President | Mahesh S N (Mahesh Gowda) | Confirmed |
| Vice President | Mallikarjuna | Confirmed |
| General Secretary | Kaarthik | Post confirmed; photo matched by message order |
| Joint Secretary | Somasundaram | Post confirmed; photo matched by message order |
| Joint Secretary | Jivan | Post confirmed; photo matched by message order |
| Joint Secretary | Bharat | Post confirmed; photo matched by message order |
| Treasurer | Nagaraj N G V | Confirmed |
| Committee member | Kalesh | To be confirmed |
| Legal Advisor | Shah | Confirmed |
| Legal Advisor | Guru Prasad | To be confirmed (KRA replied to the red t-shirt photo, sent at 4:39 pm, with "Shah and Guru Prasad legal advisor") |

- **Unconfirmed matches (internal note only):** the photos for Kaarthik, Somasundaram, Jivan and Bharat were assigned in the order they were sent on WhatsApp (4:28, 4:30, 4:32 and 4:34 pm). Kalesh was assigned to the 4:42 pm photo. The live site does NOT show any "to be confirmed" tags or mention WhatsApp, because it is a professional public site. If KRA reports a wrong match, swap the image files or names.
- **Photo crops:** faces only. The register pages behind the photos contain handwritten addresses and phone numbers, so never publish the full pages.

### KRA Election and installation ceremony 2026

- **Election:** held 25 June 2026. Ballot papers exist for President, Secretary, Joint Secretary and Treasurer.
- **Results:** the elected committee is the one in the Leadership section, and each member received an election certificate (ಪ್ರಮಾಣ ಪತ್ರ).
- **Installation ceremony (ಪದಗ್ರಹಣ ಸಮಾರಂಭ):** its programme is listed on the site. Its date is not yet known.
- **Privacy rules for this event:**
  - Do not publish ballot papers, because they show candidates who were not elected.
  - Never publish filled nomination forms, because they contain Aadhaar numbers.

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
| MG Builders | Trophy sponsor. Houses, villas and construction. No. 174, 'C' Cross, 1st Main, 7th Block, Koramangala, Bengaluru 560095. |
| Vinoth, Pelican | Proud sponsor. C.K. Plaza, Bellary Road, Gangenahalli, Bengaluru 560006. |

## Still placeholder / waiting for KRA

- KRA's own About text and the President's message (current text is a draft written for them).
- Trustee and founding member names, and the names and posts of the six unnamed first-committee members.
- Real members list (remove the `s:1` sample rows).
- Phone/WhatsApp and email (email is a placeholder until the domain exists). The office address and registration number are confirmed and already on the site.
- Social media links (footer `href="#"`).
- Documents (PDFs): registration certificate, bye-laws, circulars.
- Tournament results.
- Upcoming event details.
- An original KRA logo file. The current logo and office bearer photos are cropped from the digital tournament poster (KRA Poster.pdf).

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

1. Deliver every update as the whole folder zipped (`index.html`, `CLAUDE.md`, `images/`). The client uploads the zip contents to GitHub in one go.
2. Open `index.html` in a browser at phone width (390px) and laptop width (1280px).
3. Check that the browser console has no JS errors.
4. Check that nav links jump to the right sections, that the mobile "Menu" button opens and closes, that the gallery album buttons filter correctly, that the lightbox opens and closes (including the Esc key), and that the member search filters rows.
5. Keep the file name `index.html` and keep the `images` folder next to it. GitHub Pages needs both.
6. Test over a local web server (e.g. `python3 -m http.server`) and confirm that no image is broken.

## Tone rule

This is KRA's live public website. Never show internal notes on the page: no "to be confirmed" tags, no mentions of WhatsApp or drafts, no "matched from messages" text. Keep these notes in this file only.
