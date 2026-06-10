# Animated HTML Presentation — Reusable Prompt Template
> Based on: Photoline Field Duty Timesheet Presentation (2026-06-07)
> Output file: `Photoline_Documentation_Animated.html`

Gamitin ito as starting prompt para sa susunod na presentation.
Palitan lang ang **CONTENT** section — lahat ng animation/visual system ay mananatiling pareho.

---

```
Gumawa ng isang self-contained animated HTML presentation file (single .html file,
walang external dependencies — lahat ng libraries embedded inline).

--- CONTENT ---
Topic: [ILAGAY ANG TOPIC]
Slides: [ILAGAY ANG LISTAHAN NG SECTIONS/SLIDES]
Language: [Filipino / English / Mixed]

--- TECH STACK (i-embed lahat inline) ---
- GSAP 3.12.5 (animations + timelines)
- Three.js r134 (Vanta dependency)
- Vanta.js NET 0.5.24 (animated particle network background)
- Font: Nunito (Google Fonts, i-embed as base64 o CDN)
- Web Audio API (synthesized sound effects, zero audio files)

--- VISUAL DESIGN ---
Color tokens:
  --bg:     #060B18   (deep navy)
  --card:   #0D1B2E   (card background)
  --blue:   #2563EB   (primary)
  --cyan:   #00C8FF   (accent / glow)
  --green:  #10B981
  --red:    #EF4444
  --gold:   #F59E0B
  --white:  #F8FAFC
  --gray:   #94A3B8
  --border: rgba(37,99,235,0.25)

Typography: Nunito, weights 400–900
  - Headings: 800–900 weight
  - Body: 400–500, 14px, line-height 1.7

--- SLIDE SYSTEM ---
- Each slide = <section class="section slide" id="sec-[name]">
- position: fixed; inset: 0; height: 100vh; visibility: hidden
- Active slide: visibility: visible; z-index: 20
- Navigation: left/right arrow keys + on-screen dot indicators + slide counter (01/08)
- fitSlide() function: auto-zooms .container using CSS zoom property to fit 100vh
  (min zoom 0.7; falls back to scroll if content too long)
- Vanta NET background color changes per slide (VCOLORS array)
- Background color transitions per slide (BGCOLORS array)

--- TRANSITIONS ---
Scatter/Realign morph (between slides):
  Phase 1 — elements scatter to random positions:
    duration: 0.62s, stagger: 0.028, ease: power2.in
  Phase 2 — elements fly in and snap:
    duration: 0.78s, stagger: 0.042, ease: back.out(1.15)
  Selector (SCATTER_SEL): h2, h3, p, li, td, th, .card, .step, .badge,
    .alert, table, img, .hero-eyebrow, .hero-divider

--- INTRO SCREEN ---
- Full black background (#000000)
- Project name centered, split into individual <span class="il"> letters
- 3D letter entrance: each letter flies in from z:-700px, rotateY:-90deg,
  staggered from center, ease: back.out(1.35), delay: 0.25s
- Ambient wave float: letters ripple y:-16px, duration 2.1s, sine.inOut, yoyo repeat
- Glow pulse: blue to cyan text-shadow wave across letters
- Mouse parallax: mousemove tilts logo in 3D (28deg rotateY, 16deg rotateX)
- Cyan scan line sweeping top-to-bottom (3.4s loop) — holographic projector effect
- Hyperspace warp Canvas 2D transition on Start button click:
    750 stars, 3D perspective projected, speed * 1.065 exponential acceleration,
    motion blur via semi-transparent fills, white-out flash at speed >= 0.149
- CRITICAL: Float-safe trigger uses speed >= 0.149 (NOT t >= 1.0 — floating point bug)

--- THANK YOU SLIDE ---
- 4 expanding rings (.ty-ring) — concentric circles, cyan border, pulsing
- "Thank You" title: scale 3 to 1, blur 28px to 0, ease: power3.out
- Subtitle, divider line, "— The End —" text fading in sequence
- Section ID: sec-thankyou (last in SLIDES array)
- Special transition: fade out previous slide then dramatic entrance (no scatter)

--- SOUND EFFECTS (Web Audio API, synthesized) ---
5 functions, same call sites every time:
  SFX.warp()     — on Start button click (intro to presentation)
  SFX.powerOn()  — on hero slide entrance
  SFX.scatter()  — on slide exit (Phase 1)
  SFX.align()    — on slide entrance (Phase 2)
  SFX.thankyou() — on Thank You slide entrance

Sound theme: sci-fi binary/computer interface beeps
  - Square wave blips (instant attack, fast decay)
  - Alternating hi/lo frequencies simulating binary data transfer
  - No sustained musical chords, no bass-heavy impulses

--- HERO SLIDE ANIMATION ---
- Letters split: <span class="letter"> per character of title
- Entrance: opacity 0 to 1, y:50 to 0, blur 10px to 0, scale 0.85 to 1, stagger 0.055s
- Eyebrow text, divider, description, link cards fade/slide in via GSAP timeline
- scrambleText() on section numbers (.sec-num) when each slide enters
- Section heading underline bar animates width: 0 to 100% on entry

--- KEY JS FUNCTIONS ---
- goTo(idx)               — main slide switcher, handles BUSY lock
- fitSlide(el)            — CSS zoom auto-scale for content overflow
- playHeroEntrance()      — GSAP timeline for hero slide
- playThankyouEntrance()  — ring expand + text reveal for last slide
- scrambleText(el, text, duration) — randomized character scramble effect

--- KNOWN BUGS / GOTCHAS ---
- Float precision: (0.15 - 0.095) / 0.055 = 0.9999... in JS, never equals 1.0.
  Always use a direct threshold like speed >= 0.149 instead of a normalized t >= 1.
- Kill tweens on sec-thankyou when navigating away (gsap.killTweensOf on rings/text)
- BUSY flag must be set/released correctly or navigation locks up permanently
- CSS zoom (not transform: scale) is used for fitSlide — different box model behavior

--- FILE NOTES ---
- All libraries embedded offline (no CDN calls) — opens without internet
- File size target: under 6 MB
- Browser: Chrome / Edge latest required (CSS zoom, Web Audio API, Canvas 2D)
```
