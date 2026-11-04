# MD Pranto Ali – Portfolio Website: Complete Build Specification

> **Instruction for any AI agent:** Build a single self-contained file `index.html` (HTML + CSS + vanilla JS inline, no frameworks, no build step) that follows this document exactly. All content, structure, behavior, design tokens and data are defined below. Do not add, remove or reword content. Do **not** include any student ID number anywhere, and do **not** include a References section or any referee details.

---

## 1. Overview

- **Owner:** MD Pranto Ali (B.Sc. in CSE student, IUBAT)
- **Type:** One-page personal portfolio, 7 numbered sections + hero, with two popup (modal) views: Project details and Certificate details.
- **Tech:** Plain HTML/CSS/JS. Only external resource: Google Font **Sora** (weights 400, 500, 600, 800) with fallback `system-ui, -apple-system, "Segoe UI", sans-serif`.
- **Language:** English UI. `<html lang="en">`. Page title: `MD Pranto Ali – Portfolio`.
- **Viewport meta:** `width=device-width, initial-scale=1, viewport-fit=cover`.
- **Visual style:** Neumorphism (soft UI) with 3D tilt cards, animated reveals, a "dynamic island" navigation, day/night themes with 10 palettes each.

## 2. Section order and navigation

| # | id | Label (island menu) | Heading |
|---|----|---------------------|---------|
| – | `#home` | Home | Hero (no number) |
| 01 | `#about` | About | About me |
| 02 | `#skills` | Skills | Technical skills |
| 03 | `#journey` | Journey | Education & experience |
| 04 | `#projects` | Projects | Projects |
| 05 | `#achievements` | Awards | Achievements |
| 06 | `#goals` | Goals | Where I’m heading |
| 07 | `#contact` | Contact | Get in touch |

Between hero and About there is an animated **ribbon** (see §5.2). Footer: `© {current year} MD Pranto Ali`.

## 3. Design system

### 3.1 CSS variables
```
--bg, --ink, --accent         (set from the active theme)
--hi  = color-mix(in srgb, var(--bg), #fff 70%)   (night: #fff 7%)
--lo  = color-mix(in srgb, var(--bg), #000 16%)   (night: #000 38%)
--mute = color-mix(in srgb, var(--ink) 62%, var(--bg))
--out = 9px 9px 18px var(--lo), -9px -9px 18px var(--hi)        (raised)
--in  = inset 6px 6px 12px var(--lo), inset -6px -6px 12px var(--hi)  (pressed)
```
Everything (cards, buttons, inputs, pills) is the same background as the page, separated only by `--out` / `--in` shadows. Large display text (section numbers, hero name, contact lead) uses `color: var(--bg)` with layered `text-shadow` (extruded 3D look); the second hero line "ALI" uses `--accent`.

### 3.2 Themes (stored in `localStorage` key `pa-theme3`, wrapped in try/catch)
Format `[name, background, text, accent]`. Default mode follows `prefers-color-scheme`, default palette index 0.

**Day:** Cloud `#e0e5ec #3d4452 #516c96` · Mint `#dcece4 #2f4a3d #2e8b6a` · Peach `#f1e1d8 #5a3e33 #d9653b` · Lavender `#e4dff0 #433a5c #7b5cd6` · Sky `#d8e8f3 #2c4560 #1e88d6` · Rose `#f2dde3 #5c3342 #d6457a` · Sand `#ece4d2 #4f4630 #b8860b` · Lemon `#f0efcf #4a4a2a #8a9a00` · Aqua `#d4eeee #284e50 #0f9d9a` · Coral `#dcdfe4 #2a2f3a #e0533d`

**Night:** Midnight `#23272f #d6d9de #8fb0e3` · Indigo `#1a1d29 #dfe3f5 #6c8cff` · Forest `#1f2a25 #d7e8de #4fd1a1` · Plum `#2a2033 #eadcf5 #c77dff` · Ember `#2b2220 #f1dfd8 #ff7a45` · Ocean `#1b2a33 #d8ecf5 #3cc4e8` · Crimson `#2c1f25 #f5dce4 #ff4d7e` · Graphite `#2a2a2a #e4e4e4 #f2c14e` · Teal `#1c2b2c #d4eeee #2dd4bf` · Violet `#221f33 #e0dcf7 #a78bfa`

### 3.3 Shared components
- **`.card`:** radius 30px, padding 26px, `--out` shadow, 3D tilt on mouse hover only (±7°, pointer: fine), stronger shadow + faint accent ring on hover.
- **`.btn`:** pill (radius 40px), Sora 600 .85rem, `--out`-style small shadow; hover/active-state = `--in` and accent text; `.btn.pri` = accent background with `--bg` text.
- **Reveal animation (slow but smooth, clearly visible):** every card/heading starts hidden and offset (cards: 170px sideways with 24° rotateY, scale .92; vertical cards 110px down; headings 120px sideways) and glides in alternately from left/right over **2.3s** (opacity 1.9s) with easing `cubic-bezier(.22,.7,.25,1)` (no overshoot). It starts only when ≥20% of the element is visible (IntersectionObserver, threshold .2, rootMargin bottom −12%), so the user sees it arrive while scrolling. After ~2.5s it switches to a fast (.4s) hover transition. Pills/tags pop in over 1.3s, staggered 140ms from 600ms; project cards on page change animate 1.5s, staggered 180ms. **Do not** disable these with a blanket `prefers-reduced-motion` rule (that makes everything appear instantly); for reduced-motion only stop the continuous background loops.
- **Background decoration:** a cursor-following radial glow (mouse devices only) and two blurred floating accent orbs.
- Layout container: `main` max-width 1120px, centered, padding 96px 18px 60px.

## 4. Dynamic island navigation (fixed top, centered)
- **Mobile stability (must keep):** the island is centered with `left:0; right:0; margin:0 auto` (NOT `left:50% + translateX(-50%)`), and `html`, `body` and `main` use `overflow-x: clip; max-width:100%` so the off-screen start positions of the reveal animations can never widen the page. This stops the menu from shifting sideways on phones.
- Collapsed: 210px wide pill showing a pulsing accent dot, the **current section name** (scroll-spy) and a chevron; a thin accent progress bar at the bottom shows page scroll progress.
- Click to expand (width min(440px, 100vw − 20px)). Expanded content: a **4-column grid** of 8 icon+label links (Home, About, Skills, Journey, Projects, Awards, Goals, Contact; active one highlighted with `--in`), a **Day / Night** segmented toggle, a caption `"{Day|Night} theme N of 10 · {name}"`, and a 5×2 grid of round palette swatches (each half background colour / half accent).
- Closes on outside click, Escape, or choosing a link.

## 5. Sections – content and behavior

### 5.0 Hero `#home`
Two columns (stacks on ≤860px; the orbit goes on top).
- Small text: `HELLO, I’M MD`
- Giant 3D name (h1): line 1 `PRANTO`, line 2 `ALI` (accent colour).
- Typewriter line (loops, with blinking caret): `CSE student at IUBAT` → `Aspiring full-stack software engineer` → `Web · Automation · Database builder`.
- Paragraph: `I turn ideas into practical digital solutions: websites, automation scripts and database-driven tools.`
- Buttons: **Let’s talk** (primary, → `#contact`), **My journey** (→ `#journey`).
- Pills: `3.72 / 4.00 Diploma CGPA` · `1.5 yrs retail operations` · `IUBAT · BCSE` · a live 12-hour clock (`hh:mm AM/PM`, updates every second).
- Social icon buttons (LinkedIn, Facebook, Instagram, GitHub, WhatsApp) rendered from `CONFIG`.
- Right side "planet": dashed circular track, inset circular well, a morphing blob core (`border-radius` animation, 9s) holding the profile photo (or initials `PA` if no photo), and six orbiting round chips rotating 28s linear around it (`HTML, CSS, JS, PHP, SQL, AI`, each counter-rotated to stay upright; orbit pauses on hover).

### 5.1 Ribbon
Tilted (−2°) inset strip with an infinite marquee (26s) of: `FULL-STACK ✦ AUTOMATION ✦ DATABASES ✦ AI ✦ IoT ✦ WEB DEVELOPMENT ✦` (repeated 4×, text in extruded `--bg` style).

### 5.2 About `#about` (01)
One card with a 3-tab segmented control (sliding thumb) and panes that flip in with a 3D rotate:
1. **Story** (centered text):
   - `I’m a Computer Science and Engineering student at IUBAT – International University of Business Agriculture and Technology, with a strong passion for software development, web technologies, automation, databases and emerging technologies.`
   - `I enjoy learning new technologies, solving real-world problems and turning ideas into practical digital solutions.`
   - `I am honest and prepared to work hard for the organization to achieve my goals, and I would welcome the opportunity to help create a positive learning environment.`
   - `Hobbies: reading storybooks and learning new skills · exploring new technologies and software · listening to music.`
2. **Profile** (a shiny profile card with a diagonal light-sweep animation; header row `IUBAT` left / `PROFILE` right; **no student ID number or "ID" row anywhere**): morphing photo blob (or `PA`), name `MD Pranto Ali`, subtitle `B.Sc. in Computer Science and Engineering`, then inset rows: Program `BCSE` · Semester `Summer 2026` · Languages `Bengali, English` · Based in `Uttara, Dhaka`.
3. **Growth:** three semicircle gauges animated when the tab opens (conic-gradient driven by `@property --p`): `80%` Full-stack development · `65%` Automation · `40%` AI & IoT.

### 5.3 Skills `#skills` (02)
One card. **Isometric 3D bar towers** (accent front face, lighter top, darker side; height animates up on first view; value label above each tower): HTML **88**, CSS **85**, JS **75**, PHP **65**, Apps Script **70**, Database **72** (labels "Apps<br>Script" and "Data-<br>base" on two lines). Below, rounded tag chips:
`Web design (HTML · CSS · JS)`, `Windows XP – 11`, `Linux`, `Microsoft Office 2007/2013/2016`, `Adobe Photoshop 7 & CS6`, `Canva`, `PC hardware configuration`, `Internet, E-mail & LAN/Wireless`, `Generative AI & Prompt Engineering`, `Microsoft Excel`.

### 5.4 Journey `#journey` (03)
Vertical centered timeline (cards alternate left/right with pulsing dots; on ≤640px all cards sit on the right of a left line). Each card has a stamp pill, title, text. Order (newest first):

| Stamp | Title | Text |
|---|---|---|
| Present | B.Sc. in Computer Science and Engineering | IUBAT – International University of Business Agriculture and Technology (running) |
| 2023 | Diploma in Engineering, Computer Technology | Kamrul Islam Siddiqui Polytechnic Institute · CGPA 3.72 / 4.00 |
| Dec 2022 – May 2023 | Intern · Kodeeo Limited | 6 months internship |
| 1 yr 6 mo | Retail operations | Bullets: Entered sales data on computer systems · Maintained sales records · Assisted customers and sold products · Supported daily business and financial activities |
| 2018 | Secondary School Certificate (SSC) | Satarpara High School · Arts group · GPA 3.06 / 5.00 |

### 5.5 Projects `#projects` (04) — projects ONLY
- Data-driven: cards are generated from a `PROJECTS` array (see §6). Owner adds new projects by appending to the array.
- Grid: 2 columns (1 column ≤760px), **4 cards per page** (`PER_PAGE = 4`).
- **Card:** thumbnail area (inset, 190px high; type badge top-left; big faded index number `01…` bottom-right; project image if `img` is set, otherwise a large round emoji icon), title (accent), short description, tag chips, and the text `View details →`. The whole card is clickable/keyboard-focusable (`role="button"`, Enter/Space).
- **See more:** a button with a right arrow, placed at the **bottom-right corner** of the section; it goes to the next page. It is hidden on the last page (or when there is only one page).
- **Pagination:** round numbered buttons `1 2 3 …` centered **below**; current page uses the pressed (`--in`) style with `aria-current="page"`. Hidden when there is only one page. Changing page re-renders cards with a pop animation and smooth-scrolls to the section top.
- **Project popup (modal):** opens on card click. Two columns: **left = project image** (4:3 rounded inset box; icon placeholder if no image), **right = all details**: type chip, title, full description, features bullet list, tag chips, and **Live demo ↗ / Source code ↗** buttons (greyed out and non-clickable when the URL is empty). Becomes one column (image on top) on ≤760px.

### 5.6 Achievements `#achievements` (05) — certificates
- Generated from an `ACHIEVEMENTS` array (§7). Responsive grid `repeat(auto-fit, minmax(235px, 1fr))`, gap 28px.
- **Card:** certificate thumbnail (4:3, double-border certificate look with emoji icon + issuer; shows the image if `img` is set), title (accent), and a meta row with issuer (left) and date (right). Whole card clickable.
- **Certificate popup (modal):** **top = the full certificate image** (`width:100%`, `object-fit:contain`, max-height 62vh, rounded, raised shadow; if `img` is empty a large generated certificate placeholder is shown with icon, issuer, "Certificate", title and date), **bottom = description**: issuer chip, title (h3), date, description paragraph.

### 5.7 Goals `#goals` (06)
A **3D rotating carousel ring** of 5 cards (each 72° apart, `rotateY(i·72deg) translateZ(R)`): `</>` Full-stack development · `⚙` Software engineering · `⟳` Automation · `✦` AI · `◉` IoT (round inset icon on top, label below). Behavior: auto-rotates slowly (−0.22°/frame) unless hovered, dragged or recently interacted (3s pause); drag/swipe with inertia (pointer events, `touch-action: pan-y`); `‹` / `›` buttons snap one card (72°); ArrowLeft/ArrowRight keys when focused. Hint text between buttons: `Drag left or right to see all`. Sizes scale up at ≥641px, ≥1024px and ≥1500px (R = 128 / 165 / 235 / 330 / 360px; card sizes grow accordingly). Pauses rendering when off-screen.

### 5.8 Contact `#contact` (07)
- Lead text (extruded): `Let’s build something together.`
- Two columns (stack ≤860px):
  - **Send a message** card: form with Name, Email, Message (all required, inset inputs, radius 18px), primary button `Send message`. On submit: opens `mailto:{email}` with subject `Portfolio message from {name}` and body `{message}\n\n— {name} ({email})`; status text `Opening your email app…`.
  - **Contact information** card with icon rows: Location `House 18, Road 10, Uttara Sector 10, Dhaka-1230` · Permanent address `Hridoypur, Satarpara, Kushtia, Khulna` · Phone `+880 1979-728818` (Copy button) · Email `prantoislamnt51@gmail.com` (Copy button; button text turns to `Copied` for 1.5s). Then sub-heading `Find me online` with the social icon buttons.

## 6. Configuration and projects data (inside the script)

```js
const CONFIG = {
  photo: "<profile photo URL or empty string>",   // used in hero blob + Profile card; if empty or failing to load, initials "PA" stay
  email: "prantoislamnt51@gmail.com",
  phone: "+880 1979-728818",
  whatsapp: "8801979728818",                       // link = https://wa.me/8801979728818
  social: { linkedin:"https://www.linkedin.com", facebook:"https://www.facebook.com",
            instagram:"https://www.instagram.com", github:"https://github.com" }  // placeholders – owner replaces with real profile URLs
};
```
Social icons are inline SVGs; links open in a new tab with `rel="noopener noreferrer"`. Any user-provided text must be HTML-escaped when injected.

**PROJECTS schema:** `{title, type, icon, desc, features:[], tags:[], img, live, code}`

| # | Title | Type | Icon | Description | Features | Tags |
|---|---|---|---|---|---|---|
| 1 | Rebate Digital Poll System | Web App | 🗳️ | A secure online polling and voting platform with one-vote-per-user control, live result charts and an admin dashboard to create and close polls. | One vote per user; Live result charts; Admin dashboard to create and close polls | Apps Script, Google Sheets, JavaScript, Live Charts |
| 2 | Smart Attendance Management System | Management System | 📋 | Digital attendance tracking for students and staff with daily and monthly reports, absence summaries and one-click export. | Daily and monthly reports; Absence summaries; One-click export | Apps Script, Google Sheets, Reports, Dashboard |
| 3 | BioTap - Smart IoT Attendance Device | IoT | 👆 | A fingerprint-based IoT attendance device. Members tap a finger, the device records the time and syncs attendance to the cloud in seconds. | Fingerprint identification; Time recorded on tap; Cloud sync in seconds | ESP32, Fingerprint Sensor, Arduino C++, REST API |
| 4 | School Management System | Management System | 🏫 | All-in-one school software for student records, classes, teachers, fees, attendance and exam results, with role-based access. | Student, class and teacher records; Fees, attendance and exam results; Role-based access | Apps Script, HTML/CSS, JavaScript, Database |
| 5 | Maa Furniture - E-commerce Website | E-commerce | 🛋️ | A complete furniture online store with product catalog, categories, cart, order management and a simple admin panel. | Product catalog and categories; Shopping cart; Order management and admin panel | HTML, CSS, JavaScript, Supabase |
| 6 | Mess Manager | Web App | 🍛 | A Bengali-language mess (boarding house) management app for members, daily meals, bazar expenses and monthly hisab. | Members and daily meals; Bazar expenses; Monthly hisab, Bengali UI | Apps Script, Google Sheets, Bengali UI |
| 7 | POS Master | Web App | 🧾 | Retail point-of-sale and inventory system with fast billing, stock tracking, sales reports and printable receipts. | Fast billing; Stock tracking; Sales reports and printable receipts | Apps Script, Google Sheets, POS, Inventory |
| 8 | AB IT Smart Inventory | Web App | 📦 | A single-page inventory app for an electronics shop: products, stock-in/out, low-stock alerts and sales history. | Products and stock-in/out; Low-stock alerts; Sales history | Apps Script, Single Page App, Inventory |
| 9 | Student ID Card Generator | Tool | 🪪 | Create, preview and download print-ready student ID cards as images or PDF, right in the browser, with no server needed. | Live card preview; Download as image or PDF; Runs fully in the browser | Vanilla JS, html2canvas, jsPDF, LocalStorage |
| 10 | PassVault | Web App | 🔐 | A personal password manager for organizing and safely storing account credentials with fast search and copy. | Organized credential storage; Fast search; One-click copy | Apps Script, Security, Web App |
| 11 | PRANTO MONITOR - Website Keep-Alive | IoT | 📡 | An ESP32-S3 board that pings a website on a schedule to keep it awake, with a liquid-glass web dashboard hosted on the board itself. | Scheduled website pings; Dashboard hosted on the board; Uptime tracking | ESP32-S3, Arduino, Web Dashboard, Uptime |

All 11 have `img:""`, `live:""`, `code:""` for now (owner fills in screenshot and links later). With 11 projects and 4 per page there are 3 pages (4 + 4 + 3).

## 7. ACHIEVEMENTS data
**Schema:** `{issuer, title, date, icon, desc, img}` (`img` = full certificate image URL; empty = generated placeholder). The NSDA item is listed first.

| # | Issuer | Title | Date | Icon | Description |
|---|---|---|---|---|---|
| 1 | NSDA, Government of Bangladesh | Web Design and Development for Freelancing – Level 3 | 29 August 2026 | 🏅 | Successfully passed the assessment under the National Skills Development Authority (NSDA), Government of the People’s Republic of Bangladesh, held at Touch And Solve. |
| 2 | Google | Introduction to Generative AI Studio | September 17, 2026 | 🤖 | Completed Google’s introductory course on Generative AI Studio. |
| 3 | Microsoft | Copilot Foundation | September 18, 2026 | 🧠 | Completed the Microsoft Copilot Foundation course. |
| 4 | Microsoft | Introduction to Prompt Engineering with GitHub Copilot | September 17, 2026 | 💬 | Completed the Microsoft course on prompt engineering with GitHub Copilot. |
| 5 | Microsoft | Introduction to MS Excel | September 18, 2026 | 📊 | Completed the Microsoft introductory course on MS Excel. |
| 6 | AWS | Understanding Placement Groups | September 18, 2026 | ☁️ | Completed the AWS course on understanding Placement Groups. |
| 7 | Canva | Canva Essentials | September 10, 2026 | 🎨 | Completed the Canva Essentials course. |
| 8 | Canva | Teacher Essentials | July 31, 2024 | 🍎 | Completed the Canva Teacher Essentials course. |

## 8. Modal (popup) behavior – shared
- Overlay: fixed, full screen, blurred translucent `--bg`, content box `min(980px, 100%)`, radius 30px, raised shadow + faint accent ring, pop-in animation; inner area scrolls (`max-height: 100vh − 28px`).
- Round `✕` close button top-right. Closes on ✕, Escape, or clicking the dark backdrop. Body scroll is locked while open; focus moves to ✕ on open and returns to the card on close. `role="dialog" aria-modal="true"`.

## 9. Responsive rules (summary)
- ≤860px: hero and contact stack to one column.
- ≤760px: project grid 1 column; project modal 1 column.
- ≤640px: smaller orbit (R 122px), timeline collapses to a single left line, tower bars narrower, goals ring smaller, paddings reduced.
- ≥1024px: larger hero orbit and profile card; ≥1500px larger again. Horizontal overflow hidden; no element may cause sideways scroll.
- Respect safe-area insets (`env(safe-area-inset-top/bottom)`).

## 10. Accessibility
Visible `:focus-visible` outline (3px accent). Clickable cards are focusable with Enter/Space. Icon-only buttons have `aria-label`. Island uses `aria-expanded`. Decorative elements are `aria-hidden`. `prefers-reduced-motion` disables animations.

## 11. Source CV facts (single source of truth)
- **Name:** Md. Pranto Ali · **Email:** prantoislamnt51@gmail.com · **Phone:** +8801979728818
- **Present address:** House No-18, Road No: 10, Uttara Sector 10, Dhaka-1230 · **Permanent address:** Vill: Hridoypur, P.O: Satarpara, District: Kushtia, Division: Khulna
- **Career objective:** Interested in developing a professional career; honest and ready to work hard for the organization; wants to help create a positive learning environment.
- **Education:** B.Sc. CSE, IUBAT (running) · Diploma in Engineering (Computer Technology), Kamrul Islam Siddiqui Polytechnic Institute, 3.72/4.00, 2023 · SSC (Arts), Satarpara High School, GPA 3.06/5.00, 2018
- **Experience:** Intern, Kodeeo Limited, 6 months (Dec 2022 – May 2023); Retail operations, 1 yr 6 mo
- **Skills:** Windows XP–11, Linux; internet, e-mail, file download, LAN/wireless; Microsoft Office 2007/2013/2016; Adobe Photoshop 7, CS6; PC hardware configuration; HTML, CSS, JavaScript
- **Languages:** Bengali, English · **Hobbies:** reading storybooks and learning new skills; exploring new technologies and software; listening to music
- **Certificates:** see §7

## 12. How to edit later
- **New project:** append an object to `PROJECTS` (new pages and numbered buttons appear automatically).
- **New certificate:** append an object to `ACHIEVEMENTS`; set `img` to the certificate image file/URL.
- **Photo / contact / social links:** edit `CONFIG`.
