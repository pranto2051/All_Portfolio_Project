# MD Pranto Ali – Portfolio: Complete Build Specification (v3)

> Give this file (and, if possible, the reference `index.html`) to any AI. Following this spec exactly must reproduce the website: same layout, same look, same behaviour. Deliverable = ONE file `index.html` (HTML + CSS + JS inline, no framework, no build step).

## 1. What the site is
A single-page **"Aurora Glass"** portfolio (v3 skin: glassmorphism over an animated gradient-mesh background, with LCD / seven-segment displays) for MD Pranto Ali, CSE student (IUBAT) and aspiring full-stack engineer. Page scrolls normally. Light and dark modes. Everything is data-driven from the `CONFIG`, `PROJECTS` and `CERTS` objects at the top of the script.

## 2. Design tokens (CSS variables on `:root`)
- Light: `--bg #e4e4e7`, `--hi #ffffff`, `--lo #c4c4cb`, `--ink #3b3b40`, `--mute #85858c`, `--c (accent) #4fd8ff`, `--lcd #050506`.
- Dark (`prefers-color-scheme: dark` unless `data-theme="light"`, or `data-theme="dark"`): `--bg #2a2d33`, `--hi #363a42`, `--lo #1d1f24`, `--ink #dcdfe4`, `--mute #939aa5`.
- Shadows (v3 glass): `--out: 0 14px 34px -14px rgba(0,0,0,.4), inset 0 1px 0 rgba(255,255,255,.14)`; `--in: inset 0 2px 8px rgba(0,0,0,.26)`. Glass tokens: `--glass: color-mix(in srgb, var(--bg) 58%, transparent)`, `--glass2` (86%), `--bd: 1px solid color-mix(in srgb, var(--ink) 15%, transparent)`. `--hi/--lo` still exist (the Theme Studio derives them) but the glass skin does not need them.
- Fonts: body `var(--ff, Roboto)` 400, line-height 1.65; LCD panels `var(--df, Orbitron)`; Google Fonts loaded: Orbitron, Roboto, Poppins, Nunito, Space Grotesk, Playfair Display, Merriweather, Fira Code, Hind Siliguri, Noto Sans Bengali, Share Tech Mono, Rajdhani, Righteous, Bungee, VT323, Press Start 2P.
- Radii: cards 30px, tiles 28px, buttons 14px, LCD panels 26px, hero 40px. Container `.wrap` max-width 980px, padding 0 18px. Section padding 30px 0. `scroll-padding-top: 84px`, smooth scroll.
- Components (v3): `.card` = glass panel (`--glass` + 18px backdrop blur, `--bd` border, radius 28, gradient hairline border drawn with `::before` + mask, mouse-follow accent spotlight in `::after` using `--mx/--my`, lift + accent shadow on hover for project/cert/timeline cards). `.btn` = pill (radius 999) glass; hover = accent tint + glow; `.btn.on` = accent gradient fill with `--onc` text. `.tile` = glass square. `.lcd` = dark gradient display with accent-tinted inner glow. Inputs: glass, accent focus ring. Dialogs: 90% glass, 26px blur, pop-in animation, blurred dark backdrop.
- **Background**: `body::before` = four blurred radial blobs mixed from `--c`, `--ink`, `--lcd` that slowly drift (26s alternate); `body::after` = faint 28px dot grid masked to fade out. Both are fixed, `pointer-events:none`.
- **Nav**: floating glass pill (max-width 1040, radius 999, blur 20px), sticky at top; a 3px accent scroll-progress bar `#sp` is fixed at the very top.
- **Headings**: each `h2` is auto-numbered `01`, `02`… (CSS counter, small bordered pill in display font) followed by a fading accent hairline.
- **Hero**: padding 42px, accent-tinted glass gradient; name `h1` = 900 weight with ink→accent gradient text (uses `--ca` for contrast); role is an accent-tinted pill; portrait has a gradient ring, accent glow and a slow float animation.
- **Timeline**: vertical accent line with a glowing dot per item.
- **Reveal on scroll**: elements `h2, .card, .pill, .goals .lcd, .tags` in every section after the first fade/slide up (opacity 0 → 1, translateY 30px, 0.9s, stagger `(i%4)*0.09s`) via IntersectionObserver, then the helper class is removed. Disabled for `prefers-reduced-motion`.
- Responsive breakpoints: 760px (hamburger menu, single-column grids) and 520px (tighter padding, stats 1 column, pills 3 columns). Respect `prefers-reduced-motion`.

## 3. Page structure (top to bottom)
1. **Sticky nav**: hamburger (mobile only) · links About, Skills, Projects, Journey, Achievements, Contact (mobile: dropdown card with staggered slide-in) · right side: **Theme** button (opens Theme Studio) + Dark/Light mode button. Active link highlighted by scroll-spy.
2. **Time panel** (`#tm`, class `lcd time`, wrapped with its caption in `#tmw`; can be hidden from Theme Studio): row of SUN…SAT (current day glows), big seven-segment `h:mm` + small seconds + AM/PM, caption "Time".
3. Grid: left stack = LCD "MD PRANTO ALI" (caption Name) and LCD "CSE · IUBAT" (caption Studying); right (`#anw`, can be hidden; Name/Studying then share one row) = square tile with the analog clock SVG (`#analog`, viewBox 200×200, caption "Clock").
4. **Hero card**: portrait (`#photo`, 4:5, radius 25, source `CONFIG.photo` or embedded `PHOTO_EMBED` data URI), h1, role, intro, social icons (LinkedIn, Facebook, Instagram, GitHub, WhatsApp).
5. **Stats** (3 LCDs): 3.72 "OF 4.00 CGPA" (Diploma) · 6 "MONTHS" (Kodeeo internship) · "B.Sc. CSE" (IUBAT).
6. **Tools row**: calculator tile · "Remaining today" h/m/s countdown · colour tile (cycles 5 accents `#4fd8ff #7dff8a #ffb347 #ff7ab8 #b69bff`) · light/dark tile.
7. `#about` (about text, objective, "Currently growing in": Full-stack 80%, Automation 65%, AI & IoT 40% as 20-cell battery bars), `#details` (name, addresses, hobbies, languages), `#skills` (vertical liquid pills HTML 88, CSS 85, JavaScript 75, PHP 65, Apps Script 70, Database 72 + tag list), `#projects`, `#journey` (timeline cards: B.Sc. CSE 2023–present, Diploma 3.72/4.00 2023, SSC 3.06/5.00 2018, Kodeeo intern Dec 2022–May 2023), `#awards`, `#goals` (FULL-STACK, SOFTWARE ENG., AUTOMATION, AI, IOT), **`#themes`** (new), `#contact`, footer, back-to-top button.
8. Dialogs: calculator, project details/demo (`#pd`), Theme Studio (`#tp`), Certificate (`#cm`).
Exact text content is in Appendix A.

## 4. Seven-segment renderer
SVG per digit, viewBox 60×100, polygons a–g: a `10,3 50,3 44,11 16,11`; b `54,7 54,46 48,40 48,15`; c `54,54 54,93 48,85 48,60`; d `10,97 50,97 44,89 16,89`; e `6,54 6,93 12,85 12,60`; f `6,7 6,46 12,40 12,15`; g `10,50 16,45 44,45 50,50 44,55 16,55`. Digit map: 0 abcdef, 1 bc, 2 abdeg, 3 abcdg, 4 bcfg, 5 acdfg, 6 acdefg, 7 abc, 8 abcdefg, 9 abcdfg. Lit segments `fill: var(--c)` + glow; unlit opacity .07. Colon and dot are small squares. Whole group `skewX(-7deg)`.

## 5. NEW – Theme Studio (section `#themes` + nav "Theme" button)
A button opens a modal (`#tp`) with **6 tabs**. Changes apply live to the whole site and are saved to `localStorage` key `pp_theme` (wrapped in try/catch).
- **Themes**: 16 full presets (name, bg, ink, mute, accent, lcd): Pearl Light, Graphite Dark, Midnight Blue, Emerald, Rose Quartz, Sunset Amber, Royal Purple, Mint Fresh, Ocean Teal, Sand Dune, Crimson Night, Slate Mono, Lavender Mist, Forest Night, Coral Reef, Cyber Black (exact hex values are in `PRE` in the reference file).
- **Colours**: colour pickers for Accent/glow, Background, Text, Muted text, Display panels. Changing Background auto-derives `--hi`/`--lo` (dark bg: hi = bg+7% white, lo = bg+32% black; light bg: hi = bg+80% white, lo = bg+13% black), sets `data-theme` dark/light by luminance (<0.5 = dark) and picks readable text colours.
- **Fonts**: body font (Roboto, Poppins, Nunito, Space Grotesk, Playfair Display, Merriweather, Fira Code, Hind Siliguri, Noto Sans Bengali), display font (Orbitron, Share Tech Mono, Rajdhani, Righteous, Bungee, VT323, Fira Code, Poppins), text size 85–125% (root font-size).
- **Time style**: 30 digital styles (table 6.1). Each card is a mini time panel in that style (SUN–SAT row with the current day lit, AM, big `10:45`, small seconds `30`), cards min-width 235px.
- **Clock style**: 30 analog styles (table 6.2), each shown as a mini clock preview.
- **Remaining today**: 10 styles (table 6.3), each shown as a mini panel with h / m / s chips and, for some, the day-progress bar.
- **Show / hide switches** (NEW, `ST.hd` and `ST.ha`, 1 = hidden, default 0): at the top of the *Time style* tab a switch "Digital clock" and at the top of the *Clock style* tab a switch "Analog clock" (button `role="switch"`, `data-tg="hd|ha"`, label On/Off, pill switch). `applyShow()` sets `hidden` on `#tmw` (the whole day + time panel with its "Time" caption) and on `#anw` (the analog tile with its "Clock" caption), and toggles classes on the grid `#g3`: `nodt` (digital off → remove the grid's top margin) and `noan` (analog off → grid becomes one column and the stack of **Name** and **Studying** LCDs becomes **two columns in a single row**, falling back to one column under 400px). When both are off only the Name/Studying row remains. CSS: `[hidden]{display:none!important}`. State is saved in `pp_theme` and cleared by Reset (both shown again).
- **Contrast guard** (inside `applyAll`): ink vs background must reach 4.5:1 (else ink becomes #111 or #f5f5f5); muted text 3:1; accent vs display panel 3.5:1 (else `--lcd` flips to #050506 / #f4f4f6). It also sets `--ca` (accent readable on the page background, else ink; used for hover text, focus outline, selected outline, `.ok`) and `--onc` (black or white text for use on accent fills). Preview buttons style only direct SVG children (`.opt>svg`) so seven-segment digits keep their size.
- **Reset**: restores the original look (default light/dark by system setting, accent `#4fd8ff`, style 1 for the time, clock and remaining-today panels, default fonts, 100% size) and clears storage. The existing light/dark button and the colour tile also work and clear custom background colours.
- Implementation: state object `ST = {p,bg,ink,mute,lcd,c,ff,df,fs,dt,an,rs}` (`rs` = Remaining-today style); `applyAll()` clears inline root style then sets variables; `applyTime()` styles `#tm`; `applyRem()` styles `#rmw`; `drawAnalog(i)` rewrites `#analog`.

### 6.1 Digital time styles (`DS`, 30 styles)
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
| 21 | `"Emerald Glow"` | `"#34f5a2"` | `"#031a12"` | `""` |  |  |  |  |  |
| 22 | `"Orange Flame"` | `"#ff7a18"` | `"linear-gradient(135deg,#1a0900,#3a1300)"` | `""` |  |  |  |  |  |
| 23 | `"Royal Blue Segment"` | `"#7aa8ff"` | `"#0a1330"` | `""` |  |  | `"40px"` |  |  |
| 24 | `"Slate Poppins"` | `"#e2e8f0"` | `"#1e293b"` | `"Poppins"` | `600` |  |  |  |  |
| 25 | `"Candy Pop"` | `"#ffffff"` | `"linear-gradient(135deg,#ff5fa2,#7a5cff)"` | `"Righteous"` |  |  |  |  |  |
| 26 | `"Space Grotesk"` | `"#a5f3fc"` | `"#0b1220"` | `"Space Grotesk"` | `500` |  |  | `"1px solid #1e3a5f"` |  |
| 27 | `"Typewriter"` | `"#2b2b2b"` | `"#ece6d6"` | `"Fira Code"` | `500` | `0.85` | `"14px"` |  | `0` |
| 28 | `"Sakura Soft"` | `"#c2416b"` | `"#ffe3ec"` | `"Nunito"` | `800` |  | `"44px"` |  | `0` |
| 29 | `"Midnight Rajdhani"` | `"#00f0ff"` | `"linear-gradient(135deg,#050816,#13123a)"` | `"Rajdhani"` | `700` |  |  |  |  |
| 30 | `"Mono Chrome"` | `"#f5f5f5"` | `"linear-gradient(135deg,#2b2b2b,#0d0d0d)"` | `"Share Tech Mono"` |  | `0.85` |  | `"1px solid #555"` |  |

### 6.2 Analog clock styles (`AN`, 30 styles)
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
| 21 | `"Emerald Dial"` |  | `["#0f5a44", "#04231a"]` | `"#34d399"` | `3` | `2` | `"#bff5de"` | `2` |  | `"#d8f5e8"` | `"t"` | `"#ecfff6"` | `"#ffd23f"` | `"#34d399"` |  |  |  |
| 22 | `"Rose Gold"` |  | `["#f8d7cf", "#c98f86"]` | `"#8a5a52"` | `4` | `3` | `"#6b3f38"` | `3` | `"Playfair Display"` | `"#5a322c"` | `"t"` | `"#4a2923"` | `"#b3202a"` | `"#4a2923"` |  |  |  |
| 23 | `"Black Minimal"` | `"#0a0a0a"` |  | `"#333333"` | `2` | `1` | `"#ffffff"` |  |  |  | `"l"` | `"#ffffff"` | `"#ff4d4d"` | `"#ffffff"` |  |  |  |
| 24 | `"Sky Day"` |  | `["#bfe8ff", "#5aaee6"]` | `"#ffffff"` | `5` | `3` | `"#1b5a8a"` | `1` | `"Nunito"` | `"#134a73"` | `"b"` | `"#0e3a5c"` | `"#ff7a59"` | `"#0e3a5c"` |  |  |  |
| 25 | `"Violet Neon"` | `"#0d0720"` |  | `"#b388ff"` |  | `4` | `"#b388ff"` |  |  |  | `"l"` | `"#e5d4ff"` | `"#ff3df0"` | `"#b388ff"` | `1` | `1` |  |
| 26 | `"Vintage Cream"` | `"#f4ead2"` |  | `"#7a5c2e"` | `5` | `2` | `"#5a4220"` | `3` | `"Merriweather"` | `"#3b2a12"` | `"t"` | `"#2b1d0a"` | `"#8c1d18"` | `"#2b1d0a"` |  |  |  |
| 27 | `"Cyan Grid"` | `"#031a22"` |  | `"#00e5ff"` | `2` | `2` | `"#00e5ff"` | `2` | `"Share Tech Mono"` | `"#00e5ff"` |  | `"#baf7ff"` | `"#ffffff"` | `"#00e5ff"` | `4` | `1` |  |
| 28 | `"Lime Sport"` | `"#101810"` |  | `"#b6ff6a"` | `5` | `3` | `"#b6ff6a"` | `1` | `"Rajdhani"` | `"#b6ff6a"` | `"b"` | `"#ffffff"` | `"#b6ff6a"` | `"#b6ff6a"` |  |  |  |
| 29 | `"Pearl Soft"` |  | `["#ffffff", "#dcdfe6"]` | `"#c4c8d2"` | `5` | `1` | `"#8a90a0"` |  |  |  | `"t"` | `"#3b3f4a"` | `"#ff6b6b"` | `"#3b3f4a"` |  |  |  |
| 30 | `"Copper Steam"` |  | `["#d98c52", "#6e3a16"]` | `"#f0c9a0"` | `4` | `2` | `"#fff1de"` | `2` | `"Righteous"` | `"#fff1de"` | `"t"` | `"#fff1de"` | `"#ffe08a"` | `"#fff1de"` | `2` |  |  |

### 6.3 "Remaining today" styles (`RS`) – NEW
The tool-row panel `#rmw` (class `rem`: label "Remaining today", then `h`, `m`, `s` number chips counting down to midnight, plus a hidden day-progress bar `#rpb` = elapsed part of the day). A style sets local CSS variables on `#rmw`: `--c` (text), `--lcd` (chip background), `--mute` (label/unit colour, = text at 75%), `--ts` (size scale; chip font-size = 1.7rem × `--ts`, mobile 1.4rem × `--ts`), plus panel background/radius/border; classes `txt` (plain font digits instead of seven-segment), `pr` (show progress bar), `ng` (no glow). Keys: n name · c text colour · cb chip background · l panel background · f font ("" = seven-segment) · w weight · s size scale · r radius · b border · g:0 no glow · p:1 progress bar. Option cards use attribute `data-rm` (NOT `data-rs`, which is the Reset button).
| # | n | c | cb | l | f | w | s | r | b | g | p |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | `"Classic LCD (default)"` |  |  |  |  |  |  |  |  |  |  |
| 2 | `"Amber Retro"` | `"#ffb000"` | `"#1a0f00"` | `"#120a00"` |  |  |  |  |  |  |  |
| 3 | `"Matrix Terminal"` | `"#39ff14"` | `"#000a02"` | `"#00140a"` | `"Share Tech Mono"` |  | `0.8` |  |  |  | `1` |
| 4 | `"Ice Glass"` | `"#9be7ff"` | `"#0b1d33"` | `"linear-gradient(135deg,#0f2a47,#1b4468)"` |  |  |  | `"34px"` |  |  |  |
| 5 | `"Neon Pink"` | `"#ff3df0"` | `"#12001a"` | `"#0a0010"` |  |  |  |  |  |  | `1` |
| 6 | `"Paper Serif"` | `"#3a2a1a"` | `"#f7efe0"` | `"#e8dcc4"` | `"Playfair Display"` | `700` |  |  |  | `0` |  |
| 7 | `"Sunset Bar"` | `"#ffffff"` | `"rgba(0,0,0,.28)"` | `"linear-gradient(135deg,#ff7a18,#af002d 60%,#319197)"` | `"Righteous"` |  |  |  |  |  | `1` |
| 8 | `"Gold Luxury"` | `"#d4af37"` | `"#0b0b0b"` | `"#1a1608"` | `"Playfair Display"` | `600` |  |  | `"1px solid #d4af37"` |  |  |
| 9 | `"Aqua Soft"` | `"#0a6c7a"` | `"#d8f6f9"` | `"#bfeaf0"` | `"Nunito"` | `800` |  | `"40px"` |  | `0` | `1` |
| 10 | `"Racing Red"` | `"#ffffff"` | `"#7a0012"` | `"linear-gradient(120deg,#d90429,#2b0008)"` | `"Rajdhani"` | `700` |  |  |  |  | `1` |

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

© MD Pranto Ali

0