# MD Pranto Ali Portfolio – Website Build Specification

*Hand this document to any AI agent. It contains everything needed to rebuild the portfolio website exactly (same design, same behaviour, same content, same profile photo link).*

## 0. Instructions for the AI agent

Build ONE self-contained file named `index.html` (HTML + CSS + vanilla JavaScript in the same file). No frameworks, no build step, no libraries. The only external resource is the Google Font **Sora** (weights 400, 500, 600, 800): `https://fonts.googleapis.com/css2?family=Sora:wght@400;500;600;800&display=swap`. Follow every section below literally. Do not add, rename or remove sections. Do not add a References section. Reproduce all text exactly as written here. Keep the editable data blocks (CONFIG, PROFILE\_PHOTO, PROJECTS, CERTS, THEMES) at the places described, each with a clear comment so the owner can find and edit them later.

**Style in one sentence:** a soft neumorphic (soft-UI) personal portfolio with raised cards, inset (pressed) wells, floating controls, a day/night mode with 10 colour themes each, a radial bottom menu, 3D tilt and scroll-reveal animations.

## 1. Owner information (use exactly)

| Field | Value |
| --- | --- |
| Name | MD Pranto Ali (CV spelling: Md. Pranto Ali) |
| Headline | CSE student & aspiring full-stack software engineer |
| Short intro | Passionate about web design & development, I turn ideas into practical digital solutions and I am always ready to learn and work hard. |
| Student ID | 23303101 · BCSE · Summer 2026 |
| University | IUBAT – International University of Business Agriculture and Technology, B.Sc. in Computer Science and Engineering (running) |
| Diploma | Diploma in Engineering, Computer Technology, Kamrul Islam Siddiqui Polytechnic Institute, result 3.72 out of 4.00, year 2023 |
| SSC | Secondary School Certificate, Arts group, Satarpara High School, GPA 3.06 out of 5.00, year 2018 |
| Internship | Intern at Kodeeo Limited, 6 months (Dec 2022 – May 2023) |
| Present address | House No-18, Road No: 10, Uttara Sector 10, Dhaka-1230 |
| Permanent address | Vill: Hridoypur, P.O: Satarpara, District: Kushtia, Division: Khulna |
| Email | prantoislamnt51@gmail.com |
| Phone | +8801979728818 (WhatsApp number: 8801979728818) |
| Languages | Bengali, English |
| Hobbies | Reading storybooks and learning new skills; exploring new technologies and software; listening to music |

**Bio (About section, two paragraphs):**

1. I’m a Computer Science and Engineering student at IUBAT – International University of Business Agriculture and Technology, holding a Diploma in Computer Technology (CGPA 3.72 / 4.00). I’m very interested in developing my professional career and helping create a positive learning environment.
2. I’m honest, prepared to work hard for the organization and achieve my goals. I enjoy exploring new technologies and software, and turning ideas into practical digital solutions.

### Profile photo (IMPORTANT)

- Page link given by the owner: `https://kommodo.ai/i/J53acXZYBTZMw8ts4DLN`
- **Direct image link to use in `src`:** `https://plain-apac-prod-public.komododecks.com/202611/02/J53acXZYBTZMw8ts4DLN/image.png`
- The Kommodo link expires on **2 December 2026**. The owner may later replace it with any direct image URL, so the link must live in ONE constant only (see section 8).
- Fallback chain if the image fails: `images/profile.png`, then the plain letters `PA`.

## 2. Design system

### 2.1 Tokens (CSS custom properties on `:root`)

- `--bg` #e0e5ec, `--ink` #3d4452, `--accent` #516c96 (these three are overwritten by the active theme).
- `--hi` = `color-mix(in srgb, var(--bg), #fff 70%)` (light highlight). In dark theme: `#fff 7%`.
- `--lo` = `color-mix(in srgb, var(--bg), #000 16%)` (shadow). In dark theme: `#000 38%`.
- `--mute` = `color-mix(in srgb, var(--ink) 62%, var(--bg))`.
- `--out` (raised) = `9px 9px 18px var(--lo), -9px -9px 18px var(--hi)`.
- `--in` (pressed/inset) = `inset 6px 6px 12px var(--lo), inset -6px -6px 12px var(--hi)`.
- Register `@property --p { syntax:'<number>'; inherits:true; initial-value:0 }` (used by the skill knobs).
- The root element carries `data-mode` (`day`/`night`) and `data-theme` (`light`/`dark`). Add `@media (prefers-color-scheme: dark)` rules guarded as `:root:not([data-theme="light"])`, and repeat the dark `--hi/--lo` values under `:root[data-theme="dark"]`.
- Safe areas: `:root` has `box-sizing:border-box; padding-top/bottom: env(safe-area-inset-*)`. Viewport meta: `width=device-width, initial-scale=1, viewport-fit=cover`.

### 2.2 Global look

- Font: Sora, fallback `system-ui, -apple-system, 'Segoe UI', sans-serif`. Body line-height 1.65, `overflow-x:hidden`, smooth scrolling, colours transition 0.6s.
- Body background: colour `--bg` plus a dotted pattern `radial-gradient(var(--lo) 1.2px, transparent 1.4px)` with `background-size:26px 26px`, `background-attachment:fixed`.
- Two fixed, blurred accent orbs (`filter:blur(80px)`, opacity .3, `z-index:0`, slow float animation 14s alternate): orb 1 is 320×320 at top -90px / right -70px; orb 2 is 260×260 at bottom 6% / left -100px, delay -6s.
- `main`: relative, `max-width:1120px`, centred, padding `90px 18px calc(150px + safe-bottom)`. Every `section` has padding `34px 0` and `scroll-margin-top:60px`.
- Focus ring: `3px solid var(--accent)`, offset 3px.

### 2.3 Components

- **Card** `.card`: background `--bg`, radius 30px, shadow `--out`, padding 26px. Its transform is `perspective(900px) rotateX(var(--rx)) rotateY(var(--ry)) rotateZ(var(--rz))`. On hover: larger shadow `16px 16px 32px lo, -16px -16px 32px hi` plus a 1px ring of accent at 30% opacity, and `--rz` returns to 0.
- **Section heading** `.sh`: flex, gap 14px, font-size 1.4rem, colour accent. It begins with a big number `<b>` (2.6rem, weight 800, colour = `--bg` with embossed `text-shadow: 4px 4px 7px lo, -4px -4px 7px hi`).
- **Button** `.btn`: Sora 600 .85rem, background `--bg`, no border, radius 40px, padding 11px 18px, shadow 5px 5px 10px lo / -5px -5px 10px hi. Hover or `.on`: shadow `--in` and accent text. Active: scale .94. `.sq` makes it a 46px circle.
- **Scroll reveal** `.rv`: starts at opacity 0 with `perspective(900px) translateY(60px) rotateX(18deg) scale(.94)`. An IntersectionObserver (threshold .15) adds `.in` (full opacity, tilt variables). Stagger delay = `(index % 3) * 110ms`.
- **3D tilt** (mouse only, `(hover:hover) and (pointer:fine)`): on `pointermove` over any `.card` set `--ry = x*8deg` and `--rx = -y*8deg` (x,y = pointer offset from card centre, range -.5..+.5); reset to 0 on `pointerleave`.
- `prefers-reduced-motion`: disable all transitions/animations and show `.rv` content immediately.

## 3. Floating controls

1. **Logo** (fixed top-left, 14px from top/16px from left): square badge 46×46, radius `16px 16px 4px 16px`, accent background, text `PA` in `--bg`, shadow `--out`; beside it the bold text `Pranto Ali` (hidden below 600px). Links to `#home`.
2. **Top-right controls** (fixed): a pill button for day/night showing `☾ Night` (when in day mode) or `☀ Day` (when in night mode; the word is hidden below 600px), and a round 46px palette button with a palette icon.
3. **Theme panel** (opens under the palette button; width `min(310px, 100% - 28px)`, padding 20px, radius 26px, raised shadow): a segmented control with `☀ Day` and `☾ Night`; the theme name as an accent h3; a small caption such as `Day theme 1 of 10`; a 5-column grid of 10 round swatches (each a 135° gradient: 45% theme background, 55% accent). The selected swatch uses the inset shadow. Clicking outside closes the panel.
4. Selection is stored in `localStorage` under key `pa-theme2` as `{mode, day, night}` (index per mode), always inside try/catch. Default mode follows `prefers-color-scheme`.

### Themes: \[name, background, text, accent\]

| Day | bg / ink / accent | Night | bg / ink / accent |
| --- | --- | --- | --- |
| Cloud | #e0e5ec / #3d4452 / #516c96 | Midnight | #23272f / #d6d9de / #8fb0e3 |
| Mint | #dcece4 / #2f4a3d / #2e8b6a | Indigo | #1a1d29 / #dfe3f5 / #6c8cff |
| Peach | #f1e1d8 / #5a3e33 / #d9653b | Forest | #1f2a25 / #d7e8de / #4fd1a1 |
| Lavender | #e4dff0 / #433a5c / #7b5cd6 | Plum | #2a2033 / #eadcf5 / #c77dff |
| Sky | #d8e8f3 / #2c4560 / #1e88d6 | Ember | #2b2220 / #f1dfd8 / #ff7a45 |
| Rose | #f2dde3 / #5c3342 / #d6457a | Ocean | #1b2a33 / #d8ecf5 / #3cc4e8 |
| Sand | #ece4d2 / #4f4630 / #b8860b | Crimson | #2c1f25 / #f5dce4 / #ff4d7e |
| Lemon | #f0efcf / #4a4a2a / #8a9a00 | Graphite | #2a2a2a / #e4e4e4 / #f2c14e |
| Aqua | #d4eeee / #284e50 / #0f9d9a | Teal | #1c2b2c / #d4eeee / #2dd4bf |
| Coral | #dcdfe4 / #2a2f3a / #e0533d | Violet | #221f33 / #e0dcf7 / #a78bfa |

## 4. Radial bottom menu

- A fixed floating action button (FAB) at the bottom centre (22px above the safe area): 68px circle, accent background, a `+` icon (stroke 2.6). When open it rotates 135° (becomes an ×).
- Clicking opens a **semicircle fan** of 8 circular items (62px, raised shadow, icon 21px plus a .55rem bold label), in this order: **Home, About, Skills, Journey, Projects, Awards, Goals, Contact** (links to `#home #about #skills #journey #projects #achievements #goals #contact`).
- Layout: item `i` is placed at angle `180° + i * 180°/(n-1)` on a radius R (R = 185px on desktop, 140px below 600px width): `x = R cos(angle)`, `y = R sin(angle)` (this draws the upper half circle). Items animate from the FAB with `transform: translate(x,y) scale(1)`, spring easing `cubic-bezier(.3,1.4,.4,1)`, 0.55s, stagger `45ms * i`.
- A full-screen blurred veil (`backdrop-filter: blur(7px)`, `--bg` at 60%) appears behind; click on the veil, on an item, or the Escape key closes the menu.
- Scroll-spy: an IntersectionObserver with `rootMargin:'-45% 0px -50% 0px'` marks the item of the current section as `.on` (inset shadow, accent colour).
- Use simple stroke icons (24×24 viewBox, `stroke=currentColor`, width 2, round caps): home, person, code brackets, graduation cap, monitor, medal, target, envelope.

## 5. Page sections (in order)

### 5.1 Home (`#home`, padding-top 0)

A 12-column bento grid, gap 22px.

**Hero card** (span 12): grid `1fr auto`, gap 30px, `align-items:center`, radius `90px 30px 90px 30px`, padding `40px 52px`.

- Left: `h1` MD Pranto Ali (clamp(2rem, 5vw, 3.3rem), weight 800, accent); role line (weight 600) `CSE student & aspiring full-stack software engineer`; paragraph (muted, .95rem, max-width 50ch) with the short intro; a row of 5 social icon buttons (44px circles, raised; hover lifts 5px, rotates -8deg, turns inset): LinkedIn, Facebook, Instagram, GitHub, WhatsApp.
- Right: **the profile photo** inside an inset frame. Wrapper `.scene` 190×230px; inside `.pframe` with padding 12px, radius 34px, background `--bg`, shadow `--in`; inside it an `img` filling the frame, `object-fit:cover; object-position:center top; border-radius:24px`. The image looks like it sits inside a pressed frame. This replaces any decorative 3D cube.
- Below 860px padding becomes `34px 28px`. Below 600px the card becomes one centred column, radius `60px 24px 60px 24px`, the photo comes first (160×195px) and socials are centred.
- Place an HTML comment above the photo: `PROFILE PHOTO (Hero card, right side) – uses PROFILE_PHOTO from the JavaScript`.

**Clock card** (span 5, radius `30px 70px 30px 70px`): a 12-hour digital flip-clock. Two digit boxes (hours, minutes, 2 digits, font clamp(2.2rem, 6vw, 3.2rem), weight 800, accent, inset shadow, radius 18px, with a thin 2px line across the middle in `--lo`), a blinking colon (steps(2) 1s), and a vertical `AM/PM` label (`writing-mode: vertical-rl`). Updated every second.

**Coins** (span 7, three round cards in a row, each with an inner inset ring):

1. `3.72` / `Diploma CGPA / 4.00`
2. `6 mo` / `Internship Kodeeo Ltd.`
3. `BCSE` / `IUBAT` Below 860px both clock and coins span 12.

### 5.2 About (`#about`, number 01)

Two columns (one column below 860px), gap 24px.

- **Left: flip ID card** (height 330px, 3D flip on click/Enter/Space, 0.9s, `role=button`, `tabindex=0`). Front: a morphing blob avatar (110×110px, border-radius animating between `38% 62% 56% 44% / 44% 40% 60% 56%` and `58% 42% 40% 60% / 58% 56% 44% 42%` every 9s, raised shadow, 8px padding) containing **the profile photo** (`object-fit:cover`, fallback letters `PA`); name `MD Pranto Ali`; small `Student ID`; `23303101 · BCSE · Summer 2026` with the number in bold; bottom-left hint `Tap to flip ⟲`; a small inset slot decoration (54×14px) top-right. Back: small label `University`, `IUBAT`, `B.Sc. in Computer Science and Engineering`, label `Diploma`, `Computer Technology · CGPA 3.72 / 4.00` and `Kamrul Islam Siddiqui Polytechnic Institute`.
- Place this comment above the img: `PROFILE PHOTO (About section) – do NOT edit the link here; edit PROFILE_PHOTO near the top of the JavaScript (search 'PROFILE PHOTO LINK')`.
- **Right: bio card** (slight rotation `--rz:1deg`): the two bio paragraphs (muted .95rem), then an accent h3 `Currently growing in` and three neumorphic faders (inset track 14px high, accent fill at 55% opacity, and a 32px round raised knob with an accent centre dot; animate width/left over 1.5s when revealed): **Full-stack development 80%**, **Generative AI & Prompt Engineering 55%**, **Microsoft Excel & Copilot 60%**.

### 5.3 Technical skills (`#skills`, number 02)

One card containing:

- Auto-fit grid (`minmax(120px,1fr)`, gap `26px 16px`) of **3 circular knobs** (112px). Each knob = ring `conic-gradient(from 225deg, accent calc(var(--p)*2.7deg), var(--lo) calc(var(--p)*2.7deg) 270deg, transparent 270deg)`, an inner raised disc showing the number, and a small accent notch rotated by `calc(var(--p)*2.7deg - 135deg)`. `--p` animates from 0 to the value (1.8s) when revealed. Values: **Web Design 80**, **Operating Systems 80**, **MS Office 78**.
- Tag chips (raised, radius `12px 22px 12px 22px`, hover lifts 4px and rotates -2deg): `Windows XP – 11`, `Linux`, `MS Office 2007/2013/2016`, `Adobe Photoshop 7 & CS6`, `PC hardware configuration`, `Internet, E-mail & LAN/Wireless`. (The CV also lists web design with HTML, CSS and JavaScript.)
- Accent label `Languages` + chips `Bengali`, `English`.
- Accent label `Hobbies` + chips `Reading storybooks & learning new skills`, `Exploring new technologies & software`, `Listening to music`.

### 5.4 Education & experience (`#journey`, number 03)

A horizontal scroll-snap rail (`overflow-x:auto`, scrollbar hidden, gap 26px, items `flex: 1 0 min(330px, 84%)`). Each `.fold` card has radius `30px 30px 30px 80px`, a floating accent **stamp** pill (top -24px, right 22px), a title (accent, 1.05rem), muted text, and a large embossed number bottom-right (4rem; 3rem on mobile). Cards:

1. Stamp `Present` – **B.Sc. in Computer Science & Engineering** – IUBAT – International University of Business Agriculture and Technology – number 01
2. Stamp `2023` – **Diploma in Engineering, Computer Technology** – Kamrul Islam Siddiqui Polytechnic Institute · Result 3.72 / 4.00 – number 02 (`--rz:1deg` on the card)
3. Stamp `Dec 2022 – May 2023` – **Intern** – Kodeeo Limited · 6 months – number 03 (`--rz:-1deg`)
4. Stamp `2018` – **Secondary School Certificate (SSC)** – Satarpara High School · Arts group · GPA 3.06 / 5.00 – number 04 (`--rz:1deg`) Below the rail, only on mobile, the centred hint `← swipe →`.

### 5.5 Projects (`#projects`, number 04)

- Heading row: `04 Projects` on the left; on the far right a `See more →` button (hidden if there are 4 or fewer projects).
- Grid `repeat(auto-fill, minmax(240px, 1fr))`, gap 24px. **Default view shows 4 projects per page** (constant `PER = 4`).
- Project card (a `<button class='card pc'>`, padding `14px 14px 20px`): a 16:10 thumbnail with inset shadow and radius 20px (`object-fit:cover`, zooms 1.06 on hover); small muted tag line; accent title (1rem); muted description truncated to 80 characters plus `…`.
- **Pagination** below the grid: round 44px number buttons `1 2 3 4 …` (one per page = `ceil(count/4)`), active one inset/accent. Hidden when only one page. Changing the page scrolls to the section.
- **See more** shows ALL projects in one grid, hides the pagination and changes the label to `See less ←`; clicking again returns to paged view (page 1).
- Owner adds projects by appending an object to the `PROJECTS` array; pages and buttons are generated automatically.
- **Clicking a project opens the detail modal** (section 6): the FULL project picture first, then title, tag, description and optional `Live demo ↗` / `Source code ↗` buttons.

Data block to include (comment it: `ADD YOUR PROJECTS HERE`). The CV contains no project details, so ship 5 editable placeholders:

```
const PROJECTS=[
 {title:'Project One',tag:'Web · HTML/CSS/JS',img:'images/projects/project-1.png',desc:'Write your project description here: what it does, the problem it solves, the technologies you used and your role.',live:'',code:''},
 {title:'Project Two',tag:'Web · HTML/CSS/JS',img:'images/projects/project-2.png',desc:'Write your project description here.',live:'',code:''},
 {title:'Project Three',tag:'Web design',img:'images/projects/project-3.png',desc:'Write your project description here.',live:'',code:''},
 {title:'Project Four',tag:'Web design',img:'images/projects/project-4.png',desc:'Write your project description here.',live:'',code:''},
 {title:'Project Five',tag:'Web',img:'images/projects/project-5.png',desc:'Write your project description here.',live:'',code:''}
];
```

### 5.6 Achievements & certificates (`#achievements`, number 05)

Grid `repeat(auto-fill, minmax(250px, 1fr))`, gap 22px. Each item is a `<button class='card ac'>` (padding 18px, radius 26px): a 48px round icon (medal) on the left; on the right the issuer in small uppercase bold accent letters, the title in bold .9rem, and the date in small muted text. **Clicking opens the same detail modal** (certificate picture, then description).

Data block (comment it: `ADD YOUR CERTIFICATES HERE`; `img` is the certificate picture):

| Issuer | Title | Date | Image path | Description |
| --- | --- | --- | --- | --- |
| NSDA | Web design and development for freelancing — Level 3 | 29 August 2026 | images/certs/nsda-level3.jpg | Successfully passed the assessment under the National Skills Development Authority (NSDA), Government of the People’s Republic of Bangladesh, held at Touch And Solve. |
| Google | Introduction to Generative AI Studio | September 17, 2026 | images/certs/google-genai.jpg | Google course introducing Generative AI Studio. |
| Microsoft | Copilot Foundation | September 18, 2026 | images/certs/ms-copilot.jpg | Microsoft course covering the foundations of Copilot. |
| Microsoft | Introduction to Prompt Engineering with GitHub Copilot | September 17, 2026 | images/certs/ms-prompt.jpg | Microsoft course on prompt engineering with GitHub Copilot. |
| Microsoft | Introduction to MS Excel | September 18, 2026 | images/certs/ms-excel.jpg | Microsoft course introducing MS Excel. |
| AWS | Understanding Placement Groups | September 18, 2026 | images/certs/aws-placement.jpg | AWS course on understanding placement groups. |
| Canva | Teacher Essentials | July 31, 2024 | images/certs/canva-teacher.jpg | Canva Teacher Essentials course. |
| Canva | Canva Essentials | September 10, 2026 | images/certs/canva-essentials.jpg | Canva Essentials course. |

Modal meta line for certificates: `Issuer · Date`.

### 5.7 Where I’m heading (`#goals`, number 06)

A grid `repeat(auto-fit, minmax(150px, 1fr))`, gap 24px, of 5 **keycap buttons** (radius 26px, padding `26px 12px 30px`, bold .9rem muted text, a 3D key shadow `0 10px 0 -1px lo, 9px 18px 24px lo, -7px -7px 16px hi`; pressing moves it down 8px). Each has a 2rem symbol above the label: `</>` Full-stack development, `⚙` Software engineering, `⟳` Automation, `✦` AI, `◉` IoT. Clicking toggles an `.on` state (accent text, accent glow `0 0 26px` at 35%, glowing symbol). When revealed, the keys light up one after another (500ms + 220ms each).

### 5.8 Get in touch (`#contact`, number 07)

Two equal-height cards side by side (one column below 860px), gap 28px. **Both cards are perfectly straight (no rotation)**, padding 30px, radius 30px; the 3D mouse tilt still applies.

- **Left – `Send a message`**: a form with inputs `Your name`, `Your email` (type email), a growing textarea `Your message`, and a `Send message` button. Fields have an inset shadow, radius 18px, padding `14px 18px`. On submit: prevent default and open `mailto:prantoislamnt51@gmail.com?subject=Portfolio message from <name>&body=<message> — <name> (<email>)` (URL-encoded), and show the status text `Opening your email app…`.
- **Right – `Contact information`**: rows with a 42px round raised icon and an inset row (radius 22px): `Location` – **House 18, Road 10, Uttara Sector 10, Dhaka-1230** and a small line `Permanent: Vill: Hridoypur, P.O: Satarpara, Kushtia, Khulna`; `Phone` – +8801979728818 with a `Copy` button; `Email` – prantoislamnt51@gmail.com with a `Copy` button (the button text changes to `Copied` for 1.5s; use the Clipboard API with a textarea+`execCommand('copy')` fallback). Then a heading `Find me online` and the same 5 social icons.
- Footer: centred muted text `© <current year> MD Pranto Ali`.

## 6. Detail modal (projects and certificates share it)

- Full-screen overlay (`z-index:50`, `--bg` at 55% with `backdrop-filter: blur(8px)`), content centred, padding 18px. Opens with a pop animation (fade + slight scale from above).
- Box: `width:min(760px,100%)`, max-height `100% - 20px` with vertical scroll, background `--bg`, radius 30px, raised shadow plus a faint accent ring, padding `18px 18px 26px`.
- Close button `✕` (40px circle, top-right, `aria-label='Close'`). Closes on ✕, backdrop click and Escape. While open, lock body scroll and focus the close button. Attributes `role='dialog' aria-modal='true'`.
- Content order: **(1) the full image** inside an inset-shadow frame (padding 8px, radius 20px), `width:100%`, `max-height:60vh`, `object-fit:contain` so the WHOLE picture is visible; **(2)** title (accent, 1.25rem); **(3)** meta line (muted, .8rem, weight 600); **(4)** description (`white-space: pre-line`); **(5)** optional link buttons (`target=_blank rel='noopener noreferrer'`).
- Missing image: `onerror` swaps in a generated SVG placeholder (800×500, light grey-blue background `#d5dbe6`, dashed accent rounded border, the title in the middle and the text `Project image goes here` or `Certificate image goes here`) – applies to thumbnails as well.
- Escape all dynamic text before inserting it as HTML.

## 7. Social links and contact config

At the top of the script keep one editable block (comment it `EDIT YOUR DETAILS HERE`):

```
const CONFIG={email:'prantoislamnt51@gmail.com',phone:'+8801979728818',whatsapp:'8801979728818',
  social:{linkedin:'https://www.linkedin.com',facebook:'https://www.facebook.com',instagram:'https://www.instagram.com',github:'https://github.com'}};
```

The social URLs are placeholders the owner will replace. WhatsApp links to `https://wa.me/<whatsapp>`. Icons: LinkedIn and Facebook as filled brand glyphs, Instagram, GitHub and WhatsApp as 24×24 stroke icons. All social links open in a new tab with `rel='noopener noreferrer'` and have an `aria-label`/`title`.

## 8. Profile photo mechanism (must be exactly this)

Put this block directly above the CONFIG block, with the comments:

```
// ================================================================
// PROFILE PHOTO LINK  <-- change ONLY this link to change the profile photo
// (used in the Hero card at the top AND the About section ID card).
// Must be a direct image link.
// If it fails to load, images/profile.png is tried, then the letters 'PA'.
const PROFILE_PHOTO='https://plain-apac-prod-public.komododecks.com/202611/02/J53acXZYBTZMw8ts4DLN/image.png';
// ================================================================
document.querySelectorAll('[data-pp]').forEach(p=>{let t=0;
  p.onerror=()=>{if(!t++)p.src='images/profile.png';else p.replaceWith('PA')};p.src=PROFILE_PHOTO});
```

Both photo `<img>` tags (hero frame and About ID-card avatar) carry the attribute `data-pp` and NO `src` in the HTML. Each has an HTML comment above it as described in sections 5.1 and 5.2.

## 9. Responsive and accessibility rules

- Breakpoint 860px: hero padding reduces, clock and coins span the full width, About and Contact become one column.
- Breakpoint 600px: hero becomes a centred column with the photo first; logo text and button labels hidden; section numbers 2rem; swipe hint visible; radial menu radius 140px.
- No horizontal page scrolling; wide content scrolls inside its own container.
- Use semantic landmarks (`main`, `nav`, `section`, `footer`), `aria-label` on icon-only buttons, `aria-expanded` on the FAB and palette button, keyboard support for the flip card and modal.
- Keep the folder layout: `index.html`, `images/profile.png`, `images/projects/*`, `images/certs/*`.

## 10. Acceptance checklist

- [ ] Single `index.html`, Sora font, neumorphic style, dotted background and floating orbs.
- [ ] Day/Night toggle plus 10+10 themes, saved in localStorage (`pa-theme2`).
- [ ] Hero shows the profile photo inside an inset frame (no cube) and the 5 social icons.
- [ ] Clock, 3 coins, flip ID card with photo, 3 faders, 3 knobs, chips, languages, hobbies.
- [ ] Journey rail with 4 cards (B.Sc., Diploma, Internship, SSC).
- [ ] Projects: 4 per page, numbered pagination, See more / See less, detail modal with full image and description.
- [ ] Achievements: 8 items, each opening the modal with certificate image and description.
- [ ] Goals keycaps, contact form (mailto), two straight equal-height contact cards, copy buttons.
- [ ] 8-item radial menu with scroll-spy; Escape closes.
- [ ] Profile photo link exists only in the `PROFILE_PHOTO` constant, with the search-friendly comments.
- [ ] No References section anywhere.
- [ ] The JavaScript has no syntax errors and all text matches this document.
