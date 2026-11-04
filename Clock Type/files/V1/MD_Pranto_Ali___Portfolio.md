# MD Pranto Ali – Portfolio: Complete Build Specification (v3)

> Give this file (and, if possible, the reference `index.html`) to any AI. Following this spec exactly must reproduce the website: same layout, same look, same behaviour. Deliverable = ONE file `index.html` (HTML + CSS + JS inline, no framework, no build step).

## 1. What the site is
A single-page neumorphic ("soft UI") portfolio with an LCD / seven-segment theme for MD Pranto Ali, CSE student (IUBAT) and aspiring full-stack engineer. Page scrolls normally. Light and dark modes. Everything is data-driven from the `CONFIG`, `PROJECTS` and `CERTS` objects at the top of the script.

## 2. Design tokens (CSS variables on `:root`)
- Light: `--bg #e4e4e7`, `--hi #ffffff`, `--lo #c4c4cb`, `--ink #3b3b40`, `--mute #85858c`, `--c (accent) #4fd8ff`, `--lcd #050506`.
- Dark (`prefers-color-scheme: dark` unless `data-theme="light"`, or `data-theme="dark"`): `--bg #2a2d33`, `--hi #363a42`, `--lo #1d1f24`, `--ink #dcdfe4`, `--mute #939aa5`.
- Shadows: `--out: 9px 9px 18px var(--lo), -9px -9px 18px var(--hi)`; `--in: inset 6px 6px 12px var(--lo), inset -6px -6px 12px var(--hi)`.
- Fonts: body `var(--ff, Roboto)` 400, line-height 1.65; LCD panels `var(--df, Orbitron)`; Google Fonts loaded: Orbitron, Roboto, Poppins, Nunito, Space Grotesk, Playfair Display, Merriweather, Fira Code, Hind Siliguri, Noto Sans Bengali, Share Tech Mono, Rajdhani, Righteous, Bungee, VT323, Press Start 2P.
- Radii: cards 30px, tiles 28px, buttons 14px, LCD panels 26px, hero 40px. Container `.wrap` max-width 980px, padding 0 18px. Section padding 30px 0. `scroll-padding-top: 84px`, smooth scroll.
- Components: `.card` (bg, radius 30, `--out`, padding 26), `.lcd` (bg `--lcd`, text `--c`, glow text-shadow), `.tile` (neumorphic square button), `.btn` (radius 14, `6px 6px 12px / -6px -6px 12px` shadows; hover/active/`.on` = inset shadow + accent text).
- Responsive breakpoints: 760px (hamburger menu, single-column grids) and 520px (tighter padding, stats 1 column, pills 3 columns). Respect `prefers-reduced-motion`.

## 3. Page structure (top to bottom)
1. **Sticky nav**: hamburger (mobile only) · links About, Skills, Projects, Journey, Achievements, Contact (mobile: dropdown card with staggered slide-in) · right side: **Theme** button (opens Theme Studio) + Dark/Light mode button. Active link highlighted by scroll-spy.
2. **Time panel** (`#tm`, class `lcd time`): row of SUN…SAT (current day glows), big seven-segment `h:mm` + small seconds + AM/PM, caption "Time".
3. Grid: left stack = LCD "MD PRANTO ALI" (caption Name) and LCD "CSE · IUBAT" (caption Studying); right = square tile with the analog clock SVG (`#analog`, viewBox 200×200, caption "Clock").
4. **Hero card**: portrait (`#photo`, 4:5, radius 25, source `CONFIG.photo` or embedded `PHOTO_EMBED` data URI), h1, role, intro, social icons (LinkedIn, Facebook, Instagram, GitHub, WhatsApp).
5. **Stats** (3 LCDs): 3.72 "OF 4.00 CGPA" (Diploma) · 6 "MONTHS" (Kodeeo internship) · "B.Sc. CSE" (IUBAT).
6. **Tools row**: calculator tile · "Remaining today" h/m/s countdown · colour tile (cycles 5 accents `#4fd8ff #7dff8a #ffb347 #ff7ab8 #b69bff`) · light/dark tile.
7. `#about` (about text, objective, "Currently growing in": Full-stack 80%, Automation 65%, AI & IoT 40% as 20-cell battery bars), `#details` (name, addresses, hobbies, languages), `#skills` (vertical liquid pills HTML 88, CSS 85, JavaScript 75, PHP 65, Apps Script 70, Database 72 + tag list), `#projects`, `#journey` (timeline cards: B.Sc. CSE 2023–present, Diploma 3.72/4.00 2023, SSC 3.06/5.00 2018, Kodeeo intern Dec 2022–May 2023), `#awards`, `#goals` (FULL-STACK, SOFTWARE ENG., AUTOMATION, AI, IOT), **`#themes`** (new), `#contact`, references card, footer, back-to-top button.
8. Dialogs: calculator, project details/demo (`#pd`), Theme Studio (`#tp`), Certificate (`#cm`).
Exact text content is in Appendix A.

## 4. Seven-segment renderer
SVG per digit, viewBox 60×100, polygons a–g: a `10,3 50,3 44,11 16,11`; b `54,7 54,46 48,40 48,15`; c `54,54 54,93 48,85 48,60`; d `10,97 50,97 44,89 16,89`; e `6,54 6,93 12,85 12,60`; f `6,7 6,46 12,40 12,15`; g `10,50 16,45 44,45 50,50 44,55 16,55`. Digit map: 0 abcdef, 1 bc, 2 abdeg, 3 abcdg, 4 bcfg, 5 acdfg, 6 acdefg, 7 abc, 8 abcdefg, 9 abcdfg. Lit segments `fill: var(--c)` + glow; unlit opacity .07. Colon and dot are small squares. Whole group `skewX(-7deg)`.

## 5. NEW – Theme Studio (section `#themes` + nav "Theme" button)
A button opens a modal (`#tp`) with **5 tabs**. Changes apply live to the whole site and are saved to `localStorage` key `pp_theme` (wrapped in try/catch).
- **Themes**: 16 full presets (name, bg, ink, mute, accent, lcd): Pearl Light, Graphite Dark, Midnight Blue, Emerald, Rose Quartz, Sunset Amber, Royal Purple, Mint Fresh, Ocean Teal, Sand Dune, Crimson Night, Slate Mono, Lavender Mist, Forest Night, Coral Reef, Cyber Black (exact hex values are in `PRE` in the reference file).
- **Colours**: colour pickers for Accent/glow, Background, Text, Muted text, Display panels. Changing Background auto-derives `--hi`/`--lo` (dark bg: hi = bg+7% white, lo = bg+32% black; light bg: hi = bg+80% white, lo = bg+13% black), sets `data-theme` dark/light by luminance (<0.5 = dark) and picks readable text colours.
- **Fonts**: body font (Roboto, Poppins, Nunito, Space Grotesk, Playfair Display, Merriweather, Fira Code, Hind Siliguri, Noto Sans Bengali), display font (Orbitron, Share Tech Mono, Rajdhani, Righteous, Bungee, VT323, Fira Code, Poppins), text size 85–125% (root font-size).
- **Time style**: 20 digital styles (table 6.1). Each card is a mini time panel in that style (SUN–SAT row with the current day lit, AM, big `10:45`, small seconds `30`), cards min-width 235px.
- **Clock style**: 20 analog styles (table 6.2), each shown as a mini clock preview.
- **Contrast guard** (inside `applyAll`): ink vs background must reach 4.5:1 (else ink becomes #111 or #f5f5f5); muted text 3:1; accent vs display panel 3.5:1 (else `--lcd` flips to #050506 / #f4f4f6). It also sets `--ca` (accent readable on the page background, else ink; used for hover text, focus outline, selected outline, `.ok`) and `--onc` (black or white text for use on accent fills). Preview buttons style only direct SVG children (`.opt>svg`) so seven-segment digits keep their size.
- **Reset**: restores the original look (default light/dark by system setting, accent `#4fd8ff`, style 1 for both clocks, default fonts, 100% size) and clears storage. The existing light/dark button and the colour tile also work and clear custom background colours.
- Implementation: state object `ST = {p,bg,ink,mute,lcd,c,ff,df,fs,dt,an}`; `applyAll()` clears inline root style then sets variables; `applyTime()` styles `#tm`; `drawAnalog(i)` rewrites `#analog`.

### 6.1 Digital time styles (`DS`)
Keys: n name · c colour ("" = theme accent) · l background · f font ("" = seven-segment, else plain text digits) · w weight · s size scale · r radius · b border · g:0 = no glow. Text-mode styles use class `txt` (no skew, font inherited).
| # | n | c | l | f | w | s | r | b | g |
|---|---|---|---|---|---|---|---|---|---|
| 1 | `"Classic LCD (default)"` | `""` | `""` | `""` |  |  |  |  |  |
| 2 | `"Amber Retro"` | `"#ffb000"` | `"#1a0f00"` | `""` |  |  |  |  |  |
| 3 | `"Matrix Terminal"` | `"#39ff14"` | `"#000a02"` | `"Share Tech Mono"` |  | `0.85` |  |  |  |
| 4 | `"Ice Glass"` | `"#9be7ff"` | `"linear-gradient(135deg,#0b1d33,#1b4468)"` | `""` |  |  | `"44px"` |  |  |
| 5 | `"Pixel Arcade"` | `"#ffe600"` | `"#1a1033"` | `"Press Start 2P"` |  | `0.5` |  |  |  |
| 6 | `"Paper Serif"` | `"#3a2a1a"` | `"#f7efe0"` | `"Playfair Display"` | `700` |  |  |  | `0` |
| 7 | `"Neon Pink"` | `"#ff3df0"` | `"#12001a"` | `""` |  |  |  |  |  |
| 8 | `"Mono Thin"` | `"#ffffff"` | `"#111111"` | `"Poppins"` | `200` |  |  |  |  |
| 9 | `"Sunset Gradient"` | `"#ffffff"` | `"linear-gradient(135deg,#ff7a18,#af002d 55%,#319197)"` | `"Righteous"` |  |  |  |  |  |
| 10 | `"Cyber Purple"` | `"#b388ff"` | `"#120726"` | `""` |  |  |  |  |  |
| 11 | `"Red Alert"` | `"#ff2a2a"` | `"#140000"` | `""` |  |  |  |  |  |
| 12 | `"Ink on Cream"` | `"#222222"` | `"#f3f1ea"` | `"Merriweather"` | `700` |  |  |  | `0` |
| 13 | `"Code Editor"` | `"#7ee787"` | `"#0d1117"` | `"Fira Code"` |  | `0.85` |  | `"1px solid #30363d"` |  |
| 14 | `"Bold Block"` | `"#ffd23f"` | `"#1b1b1b"` | `"Bungee"` |  | `0.8` |  |  |  |
| 15 | `"Aqua Soft"` | `"#0a6c7a"` | `"#d8f6f9"` | `"Nunito"` | `800` |  | `"40px"` |  | `0` |
| 16 | `"Gold Luxury"` | `"#d4af37"` | `"#0b0b0b"` | `"Playfair Display"` | `600` |  |  | `"1px solid #d4af37"` |  |
| 17 | `"Terminal Cyan"` | `"#00e5ff"` | `"#00141a"` | `"VT323"` |  | `1.25` |  |  |  |
| 18 | `"Racing Red"` | `"#ffffff"` | `"linear-gradient(120deg,#d90429,#2b0008)"` | `"Rajdhani"` | `700` |  |  |  |  |
| 19 | `"Lavender Dream"` | `"#6b4fbb"` | `"#efe9ff"` | `"Poppins"` | `300` |  | `"44px"` |  | `0` |
| 20 | `"Forest Segment"` | `"#b6ff6a"` | `"linear-gradient(135deg,#0c2415,#143a22)"` | `""` |  |  |  |  |  |

### 6.2 Analog clock styles (`AN`)
Drawn by `analogSVG(cfg, live)` in a 200×200 viewBox, face circle r=95 (or rounded square when `sq`). Keys: f face · fc gradient [inner, outer] (radial, centre 38%/32%) · r ring colour · rw ring width · t ticks (1 hour dots, 2 minute lines, 3 hour bars, 4 dots + fine dots, 5 classic 8 short lines) · tc tick colour · m numerals (1 quarters 12-3-6-9, 2 all 12, 3 roman) · nf/nc numeral font/colour · h hand shape (l line, t taper, b bar) · hc hand colour · sc second-hand colour · cc centre colour · x extra (1 inner ring r=76, 2 sunburst rays, 4 triple ring) · g glow filter · sq square face. Hours hand length 46, minute 70, second 78 (tails 12/16/16). Live hands are `<g id="hh|mh|sh">` rotated every second: hour `(h%12 + m/60)*30`, minute `(m + s/60)*6`, second `s*6`. Previews use a fixed 10:10:30.
| # | n | f | fc | r | rw | t | tc | m | nf | nc | h | hc | sc | cc | x | g | sq |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | `"Classic (default)"` |  |  |  |  | `5` |  | `1` |  |  |  |  |  |  |  |  |  |
| 2 | `"Minimal Dots"` |  |  |  |  | `1` |  |  |  |  |  |  |  |  |  |  |  |
| 3 | `"Roman Elegance"` | `"#f6f0e0"` |  | `"#b8923a"` | `4` | `2` | `"#8a6d2b"` | `3` | `"Playfair Display"` | `"#3a2a10"` | `"t"` | `"#222"` | `"#b8923a"` | `"#222"` |  |  |  |
| 4 | `"Neon Ring"` | `"#05060a"` |  | `"var(--c)"` |  | `3` | `"var(--c)"` |  |  |  |  | `"var(--c)"` | `"#fff"` | `"var(--c)"` |  | `1` |  |
| 5 | `"Dark Chrome"` |  | `["#4a4e57", "#0f1012"]` | `"#c9ced6"` | `5` | `2` | `"#c9ced6"` | `2` |  | `"#e8ebf0"` | `"t"` | `"#e8ebf0"` | `"#ff5a36"` | `"#e8ebf0"` |  |  |  |
| 6 | `"Golden Sunburst"` |  | `["#ffe08a", "#c98a1b"]` | `"#7a4b00"` |  |  |  | `1` | `"Righteous"` | `"#4a2c00"` | `"t"` | `"#3a2200"` | `"#b00020"` | `"#3a2200"` | `2` |  |  |
| 7 | `"Blueprint"` | `"#0b3a6b"` |  | `"#8fd0ff"` | `2` | `2` | `"#cfeaff"` | `2` | `"Share Tech Mono"` | `"#cfeaff"` |  | `"#fff"` | `"#ffd23f"` | `"#fff"` |  |  |  |
| 8 | `"Mint Soft"` | `"#d9f5e8"` |  | `"#ffffff"` | `6` | `1` | `"#2f7d62"` | `1` | `"Nunito"` | `"#2f7d62"` | `"b"` | `"#2f7d62"` | `"#ff7a59"` | `"#2f7d62"` |  |  |  |
| 9 | `"Sakura"` | `"#ffe3ec"` |  | `"#ff8fb5"` | `4` | `4` | `"#ff8fb5"` |  |  |  | `"t"` | `"#c2416b"` | `"#7a1f43"` | `"#c2416b"` |  |  |  |
| 10 | `"Midnight Gold"` | `"#0d1020"` |  | `"#d4af37"` |  | `3` | `"#d4af37"` | `3` | `"Playfair Display"` | `"#d4af37"` | `"t"` | `"#d4af37"` | `"#ffffff"` | `"#d4af37"` | `1` |  |  |
| 11 | `"Rounded Square"` | `"var(--bg)"` |  | `"var(--c)"` | `4` | `3` | `"var(--mute)"` |  |  |  | `"b"` | `"var(--ink)"` |  | `"var(--c)"` |  |  | `1` |
| 12 | `"Bold Bars"` |  |  |  |  | `3` | `"var(--ink)"` |  |  |  | `"b"` |  |  |  |  |  |  |
| 13 | `"Retro Terminal"` | `"#001a07"` |  | `"#39ff14"` |  | `2` | `"#39ff14"` | `2` | `"VT323"` | `"#39ff14"` |  | `"#39ff14"` | `"#b6ff9a"` | `"#39ff14"` |  | `1` |  |
| 14 | `"Deep Ocean"` |  | `["#0a5a8a", "#06243d"]` | `"#7fd6ff"` |  | `1` | `"#ffffff"` |  |  |  | `"t"` | `"#ffffff"` | `"#7fffd4"` | `"#ffffff"` | `4` |  |  |
| 15 | `"Sunset Orb"` |  | `["#ff9a5a", "#c2335d"]` |  | `0` |  |  | `1` | `"Righteous"` | `"#ffffff"` | `"b"` | `"#ffffff"` | `"#ffe08a"` | `"#ffffff"` |  |  |  |
| 16 | `"Paper Mono"` | `"#fafafa"` |  | `"#222222"` |  | `2` | `"#222222"` | `2` | `"Playfair Display"` | `"#111111"` | `"t"` | `"#111111"` | `"#d00000"` | `"#111111"` |  |  |  |
| 17 | `"Aurora"` |  | `["#7a5cff", "#0fb5ae"]` | `"rgba(255,255,255,.55)"` |  | `1` | `"#ffffff"` |  |  |  | `"t"` | `"#ffffff"` | `"#ffe66d"` | `"#ffffff"` |  | `1` |  |
| 18 | `"Amber Wood"` |  | `["#f0d9b0", "#c8964f"]` | `"#5a3a1a"` | `6` | `2` | `"#5a3a1a"` | `2` | `"Merriweather"` | `"#3b2412"` | `"t"` | `"#3b2412"` | `"#a3201a"` | `"#3b2412"` |  |  |  |
| 19 | `"Carbon Racing"` | `"#111111"` |  | `"#e10600"` | `5` | `2` | `"#ffffff"` | `1` | `"Orbitron"` | `"#e10600"` | `"b"` | `"#ffffff"` | `"#e10600"` | `"#e10600"` |  |  |  |
| 20 | `"Glass Orb"` |  | `["rgba(255,255,255,.55)", "rgba(255,255,255,.08)"]` | `"var(--c)"` | `2` | `4` |  |  |  |  |  |  |  |  | `1` |  |  |

## 7. NEW – Achievements & Certificates
`#awards` shows one card per certificate (date LCD badge, title, issuer, "View certificate" button). Clicking a card (or Enter/Space) opens the modal `#cm`: **left** = certificate image (loaded from the `image` URL, click opens it full size in a new tab), **right** = date badge, Issued by, Certificate ID (only if set), About (falls back to "Certificate of completion issued by {issuer}."), skill tags, "Verify online" button (only if `verify` set). Mobile: stacked. Close by button, backdrop click or Esc.
- `image` empty → `DEMO_CERT = https://placehold.co/1000x700/png?text=Demo+Certificate`; if that URL fails to load, a built-in SVG demo certificate is used. To use a real certificate, put its URL in `image`.
- Data (`CERTS`, fields: title, issuer, date, image, id, skills[], desc, verify):
```
{title:"Web design and development for freelancing, Level 3",issuer:"National Skills Development Authority (NSDA), Government of Bangladesh",date:"29 AUG 2026",image:"",id:"",skills:["Web design","Web development","Freelancing"],desc:"Passed the assessment held at Touch And Solve.",verify:""},
 {title:"Introduction to Generative AI Studio",issuer:"Google",date:"17 SEP 2026",image:"",id:"",skills:["Generative AI"],desc:"",verify:""},
 {title:"Copilot Foundation",issuer:"Microsoft",date:"18 SEP 2026",image:"",id:"",skills:["Copilot"],desc:"",verify:""},
 {title:"Introduction to Prompt Engineering with GitHub Copilot",issuer:"Microsoft",date:"17 SEP 2026",image:"",id:"",skills:["Prompt engineering","GitHub Copilot"],desc:"",verify:""},
 {title:"Introduction to MS Excel",issuer:"Microsoft",date:"18 SEP 2026",image:"",id:"",skills:["MS Excel"],desc:"",verify:""},
 {title:"Understanding Placement Groups",issuer:"AWS",date:"18 SEP 2026",image:"",id:"",skills:["AWS"],desc:"",verify:""},
 {title:"Canva Essentials",issuer:"Canva",date:"10 SEP 2026",image:"",id:"",skills:["Canva","Design"],desc:"",verify:""},
 {title:"Teacher Essentials",issuer:"Canva",date:"31 JUL 2024",image:"",id:"",skills:["Canva"],desc:"",verify:""}
```

## 8. Other behaviours
- **Projects**: 5 projects (School Management, Shop Management, Inventory Management, Smart Attendance, Smart Door Lock & Gate Pass) as cards with preview image (default generated placeholder), type badge, 4 tech tags (+N), "Try demo". Click opens a details dialog (image left, info right); built-in interactive demos: school, shop, inventory, attendance, gate. 6 per page with "See more / Show less".
- **Calculator**: 4×4 key grid, safe evaluation (regex `^[0-9+\-*/. ]+$`).
- **Contact**: form opens `mailto:` with the message; Phone and Email rows have Copy buttons ("Copied" for 1.6 s). `CONFIG` holds email `prantoislamnt51@gmail.com`, phone `+880 1979-728818`, WhatsApp `8801979728818`, social links.
- **Photo**: `CONFIG.photo` URL, else `PHOTO_EMBED` base64 JPEG in a second script at the end.
- **Accessibility**: focus-visible outline in accent colour, aria-labels, keyboard-operable cards.

---
# Appendix A – Page content export (text of the page)

[About](#about)[Skills](#skills)[Projects](#projects)[Journey](#journey)[Achievements](#awards)[Contact](#contact)

SUNMONTUEWEDTHUFRISAT

AM

Time

MD PRANTO ALI

Name

CSE · IUBAT

Studying

Clock

Portrait of MD Pranto Ali

# MD Pranto Ali

CSE student & aspiring full-stack software engineer

I turn ideas into practical digital solutions: websites, automation scripts and database-driven tools.

OF 4.00 CGPA

Diploma

MONTHS

Kodeeo internship

B.Sc. CSE

IUBAT

Remaining today

**h****m****s**

Calculator

Colour

Theme

## About me

I’m a Computer Science and Engineering student at IUBAT – International University of Business Agriculture and Technology, with a strong passion for software development, web technologies, automation, databases and emerging technologies.

I enjoy learning new technologies, solving real-world problems and turning ideas into practical digital solutions.

### Objective

I am very much interested in developing my professional career. I would welcome the opportunity to help create a positive learning environment. I am honest and prepared to work hard for the organization and achieve my goals.

### Currently growing in

Full-stack development80%

Automation65%

AI & IoT40%

## Details

Name

Md. Pranto Ali

Present address

House 18, Road 10, Uttara Sector 10, Dhaka-1230

Permanent address

Vill: Hridoypur, P.O: Satarpara, District: Kushtia, Division: Khulna

Hobbies

Reading storybooks and learning new skills, exploring new technologies and software, listening to music

### Language skills

- Bengali
- English

## Technical skills

**88%**

HTML

**85%**

CSS

**75%**

JavaScript

**65%**

PHP

**70%**

Apps Script

**72%**

Database

Operating system

Microsoft Windows XP to Windows 11, Linux

Internet & email

Internet browsing, sending mail, downloading files, LAN and wireless networks

Application software

Microsoft Office 2007 / 2013 / 2016, Adobe Photoshop 7 and CS6, and more

Hardware

PC configuration: processor, motherboard, memory, input and output devices

Web design

HTML, CSS and JavaScript

## Projects

Click a project to see its preview and everything I used to build it.

## Education & experience

PRESENT

### B.Sc. in Computer Science and Engineering

IUBAT – International University of Business Agriculture and Technology

2023

### Diploma in Engineering, Computer Technology

Kamrul Islam Siddiqui Polytechnic Institute · Result 3.72 / 4.00 · Passing year 2023

2018

### Secondary School Certificate (S.S.C.)

Satarpara High School · Arts group · GPA 3.06 / 5.00

6 MO

### Intern, Kodeeo Limited

Six months, December 2022 – May 2023

## Achievements & certifications

## Where I’m heading

FULL-STACK

SOFTWARE ENG.

AUTOMATION

AI

IOT

## Get in touch

### Send a message

### Contact information

Location**House 18, Road 10, Uttara Sector 10, Dhaka-1230**

Phone

Email

### Find me online

### References

- **Dilip Kumar Das**, Associate Professor & Coordinator, IUBAT · [dilipkdas@iubat.edu](mailto:dilipkdas@iubat.edu)
- **Md. Min Khayer**, Lecturer, IUBAT · [minkhayer.cse@iubat.edu](mailto:minkhayer.cse@iubat.edu)

© MD Pranto Ali

0