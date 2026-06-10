# Photoline Animated Documentation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Transform the static Photoline Field Duty Timesheet documentation into a single animated HTML file using GSAP 3 + ScrollTrigger + Vanta.js NET particle background with 3D card effects and scroll-driven reveals.

**Architecture:** Single self-contained `.html` file built incrementally — shell first, then each content section, then all animations layered on top in one GSAP initialization block at the bottom. All content is lifted verbatim from `Photoline_Documentation.html`.

**Tech Stack:** GSAP 3.12 (cdnjs), ScrollTrigger plugin, Three.js r134 (Vanta dependency), Vanta.js NET 0.5.24, Inter (Google Fonts). No build step.

**Source file:** `d:\GJA-CODING\New Projects\Photoline_Documentation.html`
**Output file:** `d:\GJA-CODING\New Projects\Photoline_Documentation_Animated.html`

---

## File Structure

```
New Projects/
  Photoline_Documentation_Animated.html   ← single output file (all tasks write here)
docs/superpowers/
  specs/2026-06-06-photoline-animated-doc-design.md
  plans/2026-06-06-photoline-animated-doc.md
```

---

## Task 1: HTML Shell + CDN Imports + CSS Variables

**Files:**
- Create: `New Projects/Photoline_Documentation_Animated.html`

- [ ] **Step 1: Create the file with boilerplate**

```html
<!DOCTYPE html>
<html lang="tl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Photoline – Field Duty Timesheet Documentation</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
<!-- Three.js (required by Vanta) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r134/three.min.js"></script>
<!-- Vanta.js NET -->
<script src="https://cdn.jsdelivr.net/npm/vanta@0.5.24/dist/vanta.net.min.js"></script>
<!-- GSAP + ScrollTrigger -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<style>
/* ── TOKENS ─────────────────────────────────── */
:root {
  --bg:      #060B18;
  --card:    #0D1B2E;
  --card2:   #0A1628;
  --blue:    #2563EB;
  --cyan:    #00C8FF;
  --green:   #10B981;
  --red:     #EF4444;
  --gold:    #F59E0B;
  --white:   #F8FAFC;
  --gray:    #94A3B8;
  --border:  rgba(37,99,235,0.25);
  --glow-c:  rgba(0,200,255,0.25);
  --glow-b:  rgba(37,99,235,0.3);
}
/* ── RESET ──────────────────────────────────── */
*, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
html { scroll-behavior: smooth; }
body {
  font-family: 'Inter', sans-serif;
  background: var(--bg);
  color: var(--white);
  font-size: 14px;
  line-height: 1.7;
  overflow-x: hidden;
}
a { color: var(--cyan); text-decoration: none; }
a:hover { text-decoration: underline; }

/* ── SCROLL PROGRESS BAR ────────────────────── */
#progress-bar {
  position: fixed; top: 0; left: 0; height: 3px;
  background: linear-gradient(90deg, var(--blue), var(--cyan));
  width: 0%; z-index: 9999;
  transition: width 0.1s linear;
}

/* ── CONTENT WRAPPER ────────────────────────── */
.container { max-width: 860px; margin: 0 auto; padding: 0 32px; }
</style>
</head>
<body>
<div id="progress-bar"></div>

<!-- SECTIONS GO HERE -->

<script>
gsap.registerPlugin(ScrollTrigger);
// ANIMATIONS GO HERE (Task 8)
</script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

Open `New Projects/Photoline_Documentation_Animated.html` in Chrome. Expected: dark `#060B18` background, no errors in console.

---

## Task 2: Sticky Navigation Bar

**Files:**
- Modify: `New Projects/Photoline_Documentation_Animated.html`

- [ ] **Step 1: Add nav CSS (inside `<style>`)**

```css
/* ── NAV ────────────────────────────────────── */
#main-nav {
  position: fixed; top: 3px; left: 0; right: 0; z-index: 1000;
  background: transparent;
  transition: background 0.3s, border-bottom 0.3s;
  padding: 0;
}
#main-nav.scrolled {
  background: rgba(6,11,24,0.95);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
}
.nav-inner {
  max-width: 860px; margin: 0 auto;
  display: flex; align-items: center;
  justify-content: space-between;
  padding: 14px 32px;
  gap: 16px;
}
.nav-logo {
  font-size: 1.1em; font-weight: 900;
  color: var(--cyan); letter-spacing: 2px;
  white-space: nowrap;
}
.nav-links {
  display: flex; gap: 6px;
  overflow-x: auto; scrollbar-width: none;
}
.nav-links::-webkit-scrollbar { display: none; }
.nav-link {
  padding: 6px 14px; border-radius: 20px;
  font-size: 0.75em; font-weight: 600;
  color: var(--gray); white-space: nowrap;
  transition: color 0.2s, background 0.2s;
  cursor: pointer; border: none; background: none;
}
.nav-link:hover, .nav-link.active {
  color: var(--cyan);
  background: rgba(0,200,255,0.1);
}
```

- [ ] **Step 2: Add nav HTML (before `<!-- SECTIONS GO HERE -->`)**

```html
<nav id="main-nav">
  <div class="nav-inner">
    <span class="nav-logo">PHOTOLINE</span>
    <div class="nav-links">
      <button class="nav-link" onclick="document.getElementById('sec-overview').scrollIntoView({behavior:'smooth'})">Overview</button>
      <button class="nav-link" onclick="document.getElementById('sec-employee').scrollIntoView({behavior:'smooth'})">Employee</button>
      <button class="nav-link" onclick="document.getElementById('sec-admin').scrollIntoView({behavior:'smooth'})">Admin</button>
      <button class="nav-link" onclick="document.getElementById('sec-photo').scrollIntoView({behavior:'smooth'})">Photo</button>
      <button class="nav-link" onclick="document.getElementById('sec-sheets').scrollIntoView({behavior:'smooth'})">Sheets</button>
      <button class="nav-link" onclick="document.getElementById('sec-security').scrollIntoView({behavior:'smooth'})">Security</button>
    </div>
  </div>
</nav>
```

- [ ] **Step 3: Add nav scroll JS (inside `<script>`, before closing)**

```js
// Nav scroll state
window.addEventListener('scroll', () => {
  const nav = document.getElementById('main-nav');
  const scrolled = window.scrollY > 60;
  nav.classList.toggle('scrolled', scrolled);
  // Progress bar
  const total = document.body.scrollHeight - window.innerHeight;
  const pct = total > 0 ? (window.scrollY / total) * 100 : 0;
  document.getElementById('progress-bar').style.width = pct + '%';
});
// Active nav link via IntersectionObserver
const sections = ['sec-overview','sec-employee','sec-admin','sec-photo','sec-sheets','sec-security'];
const navLinks = document.querySelectorAll('.nav-link');
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      navLinks.forEach(l => l.classList.remove('active'));
      const idx = sections.indexOf(entry.target.id);
      if (idx >= 0) navLinks[idx].classList.add('active');
    }
  });
}, { threshold: 0.3 });
sections.forEach(id => {
  const el = document.getElementById(id);
  if (el) observer.observe(el);
});
```

- [ ] **Step 4: Verify in browser**

Scroll down (add 2000px of placeholder content temporarily). Expected: nav goes solid on scroll, progress bar fills, no console errors.

---

## Task 3: Hero Section

**Files:**
- Modify: `New Projects/Photoline_Documentation_Animated.html`

- [ ] **Step 1: Add hero CSS**

```css
/* ── HERO ───────────────────────────────────── */
#hero {
  min-height: 100vh;
  display: flex; align-items: center; justify-content: center;
  position: relative; overflow: hidden;
  padding: 100px 32px 60px;
}
#vanta-bg {
  position: absolute; inset: 0; z-index: 0;
}
.hero-content {
  position: relative; z-index: 1;
  text-align: center; max-width: 700px;
}
.hero-eyebrow {
  font-size: 0.75em; font-weight: 700;
  letter-spacing: 4px; color: var(--cyan);
  text-transform: uppercase; margin-bottom: 16px;
  opacity: 0; /* animated by GSAP */
}
.hero-title {
  font-size: clamp(3rem, 8vw, 5rem);
  font-weight: 900; letter-spacing: -2px;
  line-height: 1; color: var(--white);
  margin-bottom: 12px;
}
.hero-title span {
  display: inline-block; /* needed for GSAP letter split */
}
.hero-sub {
  font-size: 1.6em; font-weight: 700;
  color: var(--cyan); margin-bottom: 8px;
  opacity: 0;
}
.hero-desc {
  font-size: 0.9em; color: var(--gray);
  font-style: italic; margin-bottom: 40px;
  opacity: 0;
}
.hero-divider {
  width: 120px; height: 3px;
  background: linear-gradient(90deg, var(--blue), var(--cyan));
  margin: 20px auto 32px; border-radius: 2px;
  opacity: 0;
}
.hero-links {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 16px; max-width: 560px; margin: 0 auto;
  opacity: 0;
}
.hero-link-card {
  padding: 20px 24px; border-radius: 12px;
  text-align: left; border: 1px solid;
  transition: transform 0.2s, box-shadow 0.2s;
}
.hero-link-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 30px rgba(0,200,255,0.2);
}
.hero-link-card.blue {
  background: rgba(37,99,235,0.12);
  border-color: rgba(37,99,235,0.4);
}
.hero-link-card.gold {
  background: rgba(245,158,11,0.1);
  border-color: rgba(245,158,11,0.35);
}
.hero-link-card h3 {
  font-size: 0.85em; font-weight: 800;
  margin-bottom: 6px;
}
.hero-link-card.blue h3 { color: var(--cyan); }
.hero-link-card.gold h3 { color: var(--gold); }
.hero-link-card a {
  font-size: 0.8em; font-weight: 600; color: var(--cyan);
  word-break: break-all;
}
.hero-link-card p {
  font-size: 0.72em; color: var(--gray); margin-top: 4px;
}
.hero-date {
  font-size: 0.75em; color: var(--gray);
  margin-top: 32px; opacity: 0;
}
```

- [ ] **Step 2: Add hero HTML (inside `<!-- SECTIONS GO HERE -->`)**

```html
<section id="hero">
  <div id="vanta-bg"></div>
  <div class="hero-content">
    <div class="hero-eyebrow">System Documentation &amp; User Guide</div>
    <h1 class="hero-title" id="hero-title">
      <span>P</span><span>H</span><span>O</span><span>T</span><span>O</span><span>L</span><span>I</span><span>N</span><span>E</span>
    </h1>
    <div class="hero-sub">Field Duty Timesheet</div>
    <div class="hero-divider"></div>
    <p class="hero-desc">A Complete Digital Attendance System with Live Selfie, GPS, and Server Timestamp</p>
    <div class="hero-links">
      <div class="hero-link-card blue">
        <h3>&#128242; Employee App</h3>
        <a href="https://bit.ly/plineTimesheet" target="_blank">bit.ly/plineTimesheet</a>
        <p>Para sa lahat ng Field Employees</p>
      </div>
      <div class="hero-link-card gold">
        <h3>&#128202; Admin Dashboard</h3>
        <a href="https://photolinepayroll.github.io/attendance-app/admin.html" target="_blank">...attendance-app/admin.html</a>
        <p>Para sa Payroll/Admin staff only</p>
      </div>
    </div>
    <div class="hero-date">Prepared: May 2026 &nbsp;|&nbsp; Photoline</div>
  </div>
</section>
```

- [ ] **Step 3: Init Vanta.js (inside `<script>`)**

```js
// Vanta NET background
const isMobile = window.innerWidth < 768;
if (!isMobile) {
  VANTA.NET({
    el: '#vanta-bg',
    THREE: THREE,
    mouseControls: true,
    touchControls: false,
    gyroControls: false,
    color: 0x2563EB,
    backgroundColor: 0x060B18,
    points: 10,
    maxDistance: 22,
    spacing: 18,
    showDots: true,
    speed: 0.6,
  });
} else {
  document.getElementById('vanta-bg').style.background =
    'radial-gradient(ellipse at 50% 0%, rgba(37,99,235,0.3) 0%, transparent 70%)';
}
```

- [ ] **Step 4: Add hero GSAP animation (inside `<script>`, after Vanta init)**

```js
// Hero entrance animation
const heroTl = gsap.timeline({ delay: 0.3 });
heroTl
  .fromTo('.hero-eyebrow', { opacity:0, y:20 }, { opacity:1, y:0, duration:0.6 })
  .fromTo('#hero-title span', { opacity:0, y:40, filter:'blur(8px)' },
    { opacity:1, y:0, filter:'blur(0px)', stagger:0.05, duration:0.5, ease:'power3.out' }, '-=0.2')
  .fromTo('.hero-sub', { opacity:0, y:20 }, { opacity:1, y:0, duration:0.5 }, '-=0.1')
  .fromTo('.hero-divider', { opacity:0, scaleX:0 }, { opacity:1, scaleX:1, duration:0.5, transformOrigin:'left' })
  .fromTo('.hero-desc', { opacity:0, y:16 }, { opacity:1, y:0, duration:0.4 })
  .fromTo('.hero-links', { opacity:0, y:24 }, { opacity:1, y:0, duration:0.5 })
  .fromTo('.hero-date', { opacity:0 }, { opacity:1, duration:0.4 });
```

- [ ] **Step 5: Verify in browser**

Expected: letters of PHOTOLINE blur-in one by one, subtitle and cards fade up, Vanta particle network animates behind the hero. On mobile (narrow window), static gradient instead.

---

## Task 4: Shared Section Styles + Section Wrapper

**Files:**
- Modify: `New Projects/Photoline_Documentation_Animated.html`

- [ ] **Step 1: Add shared section CSS**

```css
/* ── SHARED SECTION STYLES ──────────────────── */
.section {
  padding: 80px 0;
  border-top: 1px solid var(--border);
}
.section:first-of-type { border-top: none; }

.sec-heading {
  font-size: 1.5em; font-weight: 900;
  color: var(--white); margin-bottom: 6px;
  display: flex; align-items: center; gap: 12px;
}
.sec-heading-bar {
  height: 3px; width: 0; /* animated to 100% */
  background: linear-gradient(90deg, var(--blue), var(--cyan));
  margin-bottom: 28px; border-radius: 2px;
  transform-origin: left;
}
.sec-num {
  font-size: 0.75em; font-weight: 900;
  color: var(--cyan); letter-spacing: 2px;
  opacity: 0.7;
}
.desc-text {
  color: var(--gray); margin-bottom: 24px;
  font-size: 0.9em; line-height: 1.75;
}

/* ── ALERT BOXES ────────────────────────────── */
.alert {
  border-radius: 10px; padding: 16px 20px;
  margin-bottom: 20px; border-left: 4px solid;
}
.alert.warning { background: rgba(239,68,68,0.08); border-color: var(--red); }
.alert.warning strong { color: var(--red); }
.alert.info    { background: rgba(37,99,235,0.08); border-color: var(--blue); }
.alert.info strong { color: var(--cyan); }
.alert.success { background: rgba(16,185,129,0.08); border-color: var(--green); }
.alert.success strong { color: var(--green); }
.alert.gold    { background: rgba(245,158,11,0.08); border-color: var(--gold); }
.alert.gold strong { color: var(--gold); }
.alert li { font-size: 0.85em; margin-left: 18px; margin-bottom: 5px; color: var(--gray); }
.alert p  { font-size: 0.85em; color: var(--gray); margin-top: 4px; }
.alert strong { display: block; margin-bottom: 8px; }

/* ── TABLES ─────────────────────────────────── */
.tbl { width:100%; border-collapse:collapse; margin-bottom:20px; font-size:0.85em; }
.tbl th {
  background: var(--blue); color: #fff;
  padding: 10px 14px; text-align:left;
  font-weight:700; font-size:0.8em;
  text-transform:uppercase; letter-spacing:.04em;
}
.tbl td {
  padding: 10px 14px;
  border-bottom: 1px solid rgba(255,255,255,0.05);
  color: var(--gray);
}
.tbl tr:nth-child(even) td { background: rgba(255,255,255,0.02); }
.tbl td:first-child { font-weight:600; color: var(--cyan); }
.tbl td.green { color:var(--green); font-weight:700; }
.tbl td.red   { color:var(--red);   font-weight:700; }

/* ── SUBSECTION HEADINGS ────────────────────── */
.sub-heading {
  font-size: 1em; font-weight:800;
  margin: 28px 0 12px;
}
.sub-heading.green { color: var(--green); }
.sub-heading.red   { color: var(--red); }
.sub-heading.gold  { color: var(--gold); }
.sub-heading.white { color: var(--white); }
```

- [ ] **Step 2: No browser check needed here — styles only.**

---

## Task 5: Section 1 — Overview (Feature Cards)

**Files:**
- Modify: `New Projects/Photoline_Documentation_Animated.html`

- [ ] **Step 1: Add feature card CSS**

```css
/* ── FEATURE CARDS ──────────────────────────── */
.feat-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 16px; margin-bottom: 32px;
}
.feat-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 12px; padding: 20px;
  transition: transform 0.2s, box-shadow 0.2s, border-color 0.2s;
  will-change: transform;
  cursor: default;
}
.feat-card:hover {
  border-color: rgba(0,200,255,0.5);
  box-shadow: 0 0 24px var(--glow-c);
}
.feat-icon { font-size: 1.8em; margin-bottom: 10px; }
.feat-title { font-size: 0.9em; font-weight:800; color:var(--white); margin-bottom:6px; }
.feat-desc  { font-size: 0.8em; color:var(--gray); line-height:1.5; }
```

- [ ] **Step 2: Add Section 1 HTML (after `</section>` of hero)**

```html
<section class="section" id="sec-overview">
  <div class="container">
    <p class="sec-num">01</p>
    <h2 class="sec-heading">System Overview</h2>
    <div class="sec-heading-bar"></div>
    <p class="desc-text">Ang Photoline Field Duty Timesheet ay isang digital attendance system para sa field employees. Gumagamit ito ng live selfie camera, real-time GPS location, at server-side timestamp para matiyak ang tamang at hindi madadayang pagre-record ng attendance.</p>

    <h3 class="sub-heading white">Key Features</h3>
    <div class="feat-grid">
      <div class="feat-card">
        <div class="feat-icon">&#128248;</div>
        <div class="feat-title">Live Selfie</div>
        <div class="feat-desc">Na may burned-in timestamp at GPS address sa photo mismo</div>
      </div>
      <div class="feat-card">
        <div class="feat-icon">&#128205;</div>
        <div class="feat-title">Real-time GPS</div>
        <div class="feat-desc">Kinukuha ang actual street address ng employee</div>
      </div>
      <div class="feat-card">
        <div class="feat-icon">&#128336;</div>
        <div class="feat-title">Server Timestamp</div>
        <div class="feat-desc">Galing sa Google Server — hindi madadaya kahit baguhin ang oras ng phone</div>
      </div>
      <div class="feat-card">
        <div class="feat-icon">&#9200;</div>
        <div class="feat-title">10-Second Expiry</div>
        <div class="feat-desc">May countdown ang selfie — kailangang i-submit agad</div>
      </div>
      <div class="feat-card">
        <div class="feat-icon">&#9729;</div>
        <div class="feat-title">Auto-save</div>
        <div class="feat-desc">Automatic na nase-save sa Google Sheets at Google Drive</div>
      </div>
      <div class="feat-card">
        <div class="feat-icon">&#128241;</div>
        <div class="feat-title">Mobile-friendly</div>
        <div class="feat-desc">Gumagana sa lahat ng Android at iPhone</div>
      </div>
    </div>

    <h3 class="sub-heading white">System Links</h3>
    <table class="tbl">
      <thead><tr><th>Page</th><th>Link</th><th>Para Kanino</th></tr></thead>
      <tbody>
        <tr><td>Employee App</td><td><a href="https://bit.ly/plineTimesheet" target="_blank">bit.ly/plineTimesheet</a></td><td>Lahat ng Field Employees</td></tr>
        <tr><td>Admin Dashboard</td><td><a href="https://photolinepayroll.github.io/attendance-app/admin.html" target="_blank">...attendance-app/admin.html</a></td><td>Admin / Payroll Officer</td></tr>
      </tbody>
    </table>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Open file. Scroll past hero. Expected: 6 feature cards visible in grid, table renders correctly, dark card styling.

---

## Task 6: Section 2 — Employee Guide (Steps)

**Files:**
- Modify: `New Projects/Photoline_Documentation_Animated.html`

- [ ] **Step 1: Add step CSS**

```css
/* ── STEPS ──────────────────────────────────── */
.steps { display:flex; flex-direction:column; gap:10px; margin-bottom:24px; }
.step  { display:flex; align-items:flex-start; gap:14px; }
.step-num {
  min-width:36px; height:36px; border-radius:50%;
  display:flex; align-items:center; justify-content:center;
  font-weight:900; font-size:0.95em; color:#fff;
  flex-shrink:0; position:relative; overflow:hidden;
}
.step-num.green { background: var(--green); box-shadow: 0 0 12px rgba(16,185,129,0.4); }
.step-num.red   { background: var(--red);   box-shadow: 0 0 12px rgba(239,68,68,0.4); }
.step-body {
  background: var(--card); border:1px solid var(--border);
  border-radius:8px; padding:12px 16px; flex:1;
}
.step-body strong { color:var(--white); font-size:0.9em; display:block; margin-bottom:3px; }
.step-body span   { color:var(--gray);  font-size:0.8em; }
.step-body span strong { display:inline; color:var(--cyan); }

/* ── FAQ ────────────────────────────────────── */
.faq-item { margin-bottom:8px; border-radius:8px; overflow:hidden; border:1px solid var(--border); }
.faq-q { background:rgba(37,99,235,0.12); padding:10px 16px; font-weight:700; color:var(--cyan); font-size:0.85em; }
.faq-a { background:var(--card); padding:10px 16px; color:var(--gray); font-size:0.82em; }
```

- [ ] **Step 2: Add Section 2 HTML**

```html
<section class="section" id="sec-employee">
  <div class="container">
    <p class="sec-num">02</p>
    <h2 class="sec-heading">Employee Guide — Log In &amp; Log Out</h2>
    <div class="sec-heading-bar"></div>
    <p class="desc-text">Sundan ang mga hakbang na ito bawat araw ng trabaho. Kailangan ng internet connection at pahintulot sa Camera at Location.</p>

    <div class="alert warning">
      <strong>&#9888;&#65039; BASAHIN BAGO GAMITIN:</strong>
      <ul>
        <li>I-open ang link sa <strong>Chrome o Safari browser</strong> — HINDI sa loob ng Messenger o Viber</li>
        <li>I-allow ang <strong>Camera</strong> at <strong>Location</strong> permission pagkatapos buksan ang link</li>
        <li>Mag-submit agad pagkatapos kumuha ng selfie — may <strong>10 seconds</strong> lang!</li>
        <li>Siguraduhing may <strong>internet connection</strong> bago mag-log</li>
      </ul>
    </div>

    <h3 class="sub-heading green">&#10003; LOG IN — Pag Pasok</h3>
    <div class="steps" id="steps-login">
      <div class="step"><div class="step-num green">1</div><div class="step-body"><strong>Buksan ang App</strong><span>I-type o i-click: <strong>bit.ly/plineTimesheet</strong> sa Chrome o Safari</span></div></div>
      <div class="step"><div class="step-num green">2</div><div class="step-body"><strong>I-allow ang Permissions</strong><span>Kapag humingi ng Camera at Location permission — i-tap ang "Allow"</span></div></div>
      <div class="step"><div class="step-num green">3</div><div class="step-body"><strong>I-type ang Full Name</strong><span>I-type ang iyong buong pangalan — isang beses lang, mase-save na para sa susunod</span></div></div>
      <div class="step"><div class="step-num green">4</div><div class="step-body"><strong>I-type ang Destination</strong><span>Ilagay ang iyong destinasyon o lokasyon ng trabaho (hal. Main Office, Site A, Warehouse)</span></div></div>
      <div class="step"><div class="step-num green">5</div><div class="step-body"><strong>I-tap ang LOG IN button</strong><span>I-tap ang berdeng "✓ Log In" button — lalabas ang selfie section</span></div></div>
      <div class="step"><div class="step-num green">6</div><div class="step-body"><strong>Mag-take ng Selfie</strong><span>I-tap ang "Open Camera" — i-tap ang shutter button para kumuha ng selfie</span></div></div>
      <div class="step"><div class="step-num green">7</div><div class="step-body"><strong>I-submit agad! (10 seconds)</strong><span>May countdown na lalabas — i-tap ang "Submit → Log In" bago mag-expire</span></div></div>
      <div class="step"><div class="step-num green">8</div><div class="step-body"><strong>I-screenshot</strong><span>I-screenshot ang "Logged In!" success screen para sa iyong personal na reference</span></div></div>
    </div>

    <h3 class="sub-heading red">&#128682; LOG OUT — Pag-alis</h3>
    <div class="steps" id="steps-logout">
      <div class="step"><div class="step-num red">1</div><div class="step-body"><strong>Buksan ang App</strong><span>I-type o i-click: <strong>bit.ly/plineTimesheet</strong></span></div></div>
      <div class="step"><div class="step-num red">2</div><div class="step-body"><strong>I-tap ang LOG OUT button</strong><span>I-tap ang pulang "&#128682; Log Out" button — lalabas ang selfie section</span></div></div>
      <div class="step"><div class="step-num red">3</div><div class="step-body"><strong>I-type ang Destination</strong><span>Ilagay ang iyong destinasyon</span></div></div>
      <div class="step"><div class="step-num red">4</div><div class="step-body"><strong>Mag-take ng Selfie</strong><span>I-tap ang "Open Camera" — kumuha ng selfie</span></div></div>
      <div class="step"><div class="step-num red">5</div><div class="step-body"><strong>I-submit agad! (10 seconds)</strong><span>I-tap ang "Submit → Log Out" bago mag-expire ang countdown</span></div></div>
      <div class="step"><div class="step-num red">6</div><div class="step-body"><strong>I-screenshot</strong><span>I-screenshot ang "Logged Out!" success screen para sa record</span></div></div>
    </div>

    <div class="alert info">
      <strong>&#128161; Tungkol sa 10-Second Timer:</strong>
      <p>Pagkatapos kumuha ng selfie, may <strong>10 seconds</strong> kang i-submit. Kung hindi ka naka-submit sa loob ng 10 segundo, mag-e-expire ang photo at kailangan mong kumuha muli ng selfie.</p>
    </div>

    <h3 class="sub-heading white">Mga Madalas na Tanong (FAQ)</h3>
    <div class="faq-item"><div class="faq-q">&#10060; Hindi gumagana ang camera</div><div class="faq-a">I-open sa Chrome o Safari — HINDI sa loob ng Messenger/Viber. I-tap ang ⋮ → Open in Chrome.</div></div>
    <div class="faq-item"><div class="faq-q">&#10060; Hindi nag-a-allow ng location</div><div class="faq-a">Browser settings → Site Settings → Location → Allow. Tapos i-refresh ang page.</div></div>
    <div class="faq-item"><div class="faq-q">&#10060; Hindi mapindot ang Log In/Out button</div><div class="faq-a">Siguraduhing naka-fill ang Full Name at Destination field bago i-tap ang button.</div></div>
    <div class="faq-item"><div class="faq-q">&#10060; Nakalimutan ang Full Name / Gusto ko itong baguhin</div><div class="faq-a">I-tap ang ✏️ Edit button sa tabi ng Full Name field para mabago ang pangalan.</div></div>
    <div class="faq-item"><div class="faq-q">&#10060; Na-expire ang photo bago maka-submit</div><div class="faq-a">Normal lang — kumuha ulit ng bagong selfie at i-submit agad sa loob ng 10 segundo.</div></div>
    <div class="faq-item"><div class="faq-q">&#10060; Hindi nag-load ang GPS address</div><div class="faq-a">Hintayin ng 5-10 segundo para makuha ang GPS signal. Siguraduhing naka-allow ang Location.</div></div>
    <div class="faq-item"><div class="faq-q">&#10060; Mali ang naka-capture na selfie</div><div class="faq-a">I-tap ang "Retake Selfie" button bago mag-expire ang countdown.</div></div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Scroll to Employee Guide. Expected: green steps for Log In, red for Log Out, warning alert renders in red-tinted box, FAQ items render.

---

## Task 7: Sections 3–6 + Footer

**Files:**
- Modify: `New Projects/Photoline_Documentation_Animated.html`

- [ ] **Step 1: Add password box CSS**

```css
/* ── PASSWORD BOX ───────────────────────────── */
.pass-box {
  background: rgba(245,158,11,0.08);
  border: 2px solid rgba(245,158,11,0.4);
  border-radius: 12px; padding: 20px 24px; margin-bottom:20px;
}
.pass-label { font-size:0.75em; font-weight:800; color:var(--gold); text-transform:uppercase; letter-spacing:.05em; }
.pass-value { font-size:2em; font-weight:900; color:var(--gold); font-family:monospace; letter-spacing:3px; margin:8px 0; }
.pass-warn  { font-size:0.8em; color:var(--red); font-weight:600; margin-top:6px; }

/* ── FOOTER ─────────────────────────────────── */
footer {
  text-align:center; padding:40px 32px;
  border-top:1px solid var(--border);
  color:var(--gray); font-size:0.8em; line-height:2;
}
footer strong { color:var(--cyan); }
```

- [ ] **Step 2: Add Sections 3–6 + Footer HTML**

```html
<!-- SECTION 3: ADMIN -->
<section class="section" id="sec-admin">
  <div class="container">
    <p class="sec-num">03</p>
    <h2 class="sec-heading">Admin Guide — Dashboard</h2>
    <div class="sec-heading-bar"></div>
    <p class="desc-text">Ang Admin Dashboard ay para sa Payroll Officer o Admin na nag-mo-monitor ng attendance ng lahat ng employees. Pwedeng mag-access ang multiple staff sabay-sabay.</p>

    <div class="alert info">
      <strong>&#128202; Admin Dashboard Link:</strong>
      <p><a href="https://photolinepayroll.github.io/attendance-app/admin.html" target="_blank">https://photolinepayroll.github.io/attendance-app/admin.html</a></p>
    </div>

    <div class="pass-box">
      <div class="pass-label">&#128274; Admin Password</div>
      <div class="pass-value">Photoline.201</div>
      <div class="pass-warn">&#9888;&#65039; Huwag ibahagi ang password sa mga employees.</div>
    </div>

    <h3 class="sub-heading white">Features ng Admin Dashboard</h3>
    <div class="feat-grid">
      <div class="feat-card"><div class="feat-icon">&#128202;</div><div class="feat-title">Attendance Table</div><div class="feat-desc">Lahat ng records na may photo thumbnail</div></div>
      <div class="feat-card"><div class="feat-icon">&#128248;</div><div class="feat-title">Photo Viewer</div><div class="feat-desc">I-click ang photo para makita ang buong selfie</div></div>
      <div class="feat-card"><div class="feat-icon">&#128205;</div><div class="feat-title">Location Verification</div><div class="feat-desc">I-click ang address para buksan ang OpenStreetMap</div></div>
      <div class="feat-card"><div class="feat-icon">&#128269;</div><div class="feat-title">Search &amp; Filter</div><div class="feat-desc">Filter by name, destination, type, at date</div></div>
      <div class="feat-card"><div class="feat-icon">&#128100;</div><div class="feat-title">Employee Summary</div><div class="feat-desc">Lalabas kapag nag-search ng specific na pangalan</div></div>
      <div class="feat-card"><div class="feat-icon">&#11015;&#65039;</div><div class="feat-title">Export CSV</div><div class="feat-desc">I-download ang records bilang Excel-compatible file</div></div>
    </div>

    <h3 class="sub-heading white">Employee Summary Cards</h3>
    <table class="tbl">
      <thead><tr><th>Card</th><th>Kahulugan</th><th>Time Range</th></tr></thead>
      <tbody>
        <tr><td>Total Days</td><td>Bilang ng unique na araw na nag-log</td><td>—</td></tr>
        <tr><td>Total Records</td><td>Kabuuang bilang ng Log In + Log Out</td><td>—</td></tr>
        <tr><td>Locations</td><td>Bilang ng unique na destinations</td><td>—</td></tr>
        <tr><td>Day Logs</td><td>Mga log sa araw</td><td>7:00 AM – 5:59 PM</td></tr>
        <tr><td>Evening Logs</td><td>Mga log sa gabi</td><td>6:00 PM – 10:00 PM</td></tr>
        <tr><td>Overtime Logs</td><td>Mga overtime na log</td><td>10:01 PM – 6:59 AM</td></tr>
      </tbody>
    </table>

    <h3 class="sub-heading white">CSV Export Format</h3>
    <table class="tbl">
      <thead><tr><th>Col</th><th>Header</th><th>Sample Data</th></tr></thead>
      <tbody>
        <tr><td>1</td><td>Name</td><td>Gilbert Alontaga</td></tr>
        <tr><td>2</td><td>Destination</td><td>Main Office</td></tr>
        <tr><td>3</td><td>Date</td><td>05/26/2026</td></tr>
        <tr><td>4</td><td>Type</td><td>IN / OUT</td></tr>
        <tr><td>5</td><td>Time</td><td>09:01 AM</td></tr>
        <tr><td>6</td><td>Address</td><td>Catmon Street, San Francisco del Monte, QC</td></tr>
        <tr><td>7</td><td>Photo Link</td><td>drive.google.com/... (clickable link)</td></tr>
      </tbody>
    </table>
  </div>
</section>

<!-- SECTION 4: PHOTO DETAILS -->
<section class="section" id="sec-photo">
  <div class="container">
    <p class="sec-num">04</p>
    <h2 class="sec-heading">Selfie Photo Details</h2>
    <div class="sec-heading-bar"></div>
    <p class="desc-text">Ang bawat selfie ay may sumusunod na information na naka-burned sa larawan — hindi na mababago o matatanggal pagkatapos ma-capture:</p>
    <table class="tbl">
      <thead><tr><th>Lokasyon sa Photo</th><th>Information</th><th>Color</th></tr></thead>
      <tbody>
        <tr><td>TOP – Center</td><td>Date: MM/DD/YYYY</td><td style="color:var(--gold);font-weight:700">Yellow – malaki</td></tr>
        <tr><td>TOP – Kaliwa</td><td>Employee Full Name</td><td>White – bold</td></tr>
        <tr><td>TOP – Kaliwa (ibaba)</td><td>Destination / Location</td><td style="color:#60a5fa;font-weight:700">Light Blue</td></tr>
        <tr><td>TOP – Kanan</td><td>LOG IN o LOG OUT badge</td><td class="green">Green / Red badge</td></tr>
        <tr><td>BABA – Kaliwa (malaki)</td><td>Time: HH:MM AM/PM</td><td>White – malaki</td></tr>
        <tr><td>BABA – Kaliwa</td><td>Date: Day MM/DD/YYYY</td><td>Light gray</td></tr>
        <tr><td>BABA – Kanan</td><td>&#128205; Actual GPS Address</td><td class="green">Green</td></tr>
      </tbody>
    </table>
    <h3 class="sub-heading white">10-Second Submission Timer</h3>
    <div class="alert success">
      <strong>&#9200; Paano gumagana ang timer:</strong>
      <ul>
        <li>Pagkatapos kumuha ng selfie — magsisimula ang <strong>10-second countdown</strong> sa submit button</li>
        <li>Kapag nag-tap ng Submit bago mag-expire — <strong>titigil agad ang countdown</strong>, magpo-proceed sa submission</li>
        <li>Kapag nag-expire (0 seconds) — <strong>mabubura ang photo</strong>, kailangan kumuha muli ng selfie</li>
        <li>Sa bawat retake — <strong>bagong timestamp</strong> ang makukuha sa photo</li>
      </ul>
    </div>
  </div>
</section>

<!-- SECTION 5: GOOGLE SHEETS -->
<section class="section" id="sec-sheets">
  <div class="container">
    <p class="sec-num">05</p>
    <h2 class="sec-heading">Google Sheets Database</h2>
    <div class="sec-heading-bar"></div>
    <p class="desc-text">Ang bawat attendance log ay automatic na nase-save sa Google Sheets at ang selfie photo ay nag-u-upload sa Google Drive.</p>
    <table class="tbl">
      <thead><tr><th>Column</th><th>Header</th><th>Nilalaman</th></tr></thead>
      <tbody>
        <tr><td>A</td><td>Name</td><td>Pangalan ng employee</td></tr>
        <tr><td>B</td><td>Destination</td><td>Destinasyon o lokasyon ng trabaho</td></tr>
        <tr><td>C</td><td>Type</td><td>Log In o Log Out</td></tr>
        <tr><td>D</td><td>Timestamp</td><td>Petsa at oras (Philippine Time — mula sa Google Server)</td></tr>
        <tr><td>E</td><td>Address</td><td>Actual na street address mula sa GPS</td></tr>
        <tr><td>F</td><td>Latitude</td><td>GPS Latitude coordinates</td></tr>
        <tr><td>G</td><td>Longitude</td><td>GPS Longitude coordinates</td></tr>
        <tr><td>H</td><td>Photo Link</td><td>Link ng selfie photo sa Google Drive</td></tr>
      </tbody>
    </table>
    <div class="alert info">
      <strong>&#128194; Google Drive — "Attendance Photos" Folder:</strong>
      <p>Ang lahat ng selfie photos ay naka-save dito. Ang filename ng bawat photo ay: <strong>[Name]_[Date]-[Time].jpg</strong></p>
      <p style="margin-top:6px">Halimbawa: <code style="background:rgba(255,255,255,0.08);padding:2px 8px;border-radius:4px;color:var(--cyan)">Gilbert_2026-05-26-09-01.jpg</code></p>
    </div>
  </div>
</section>

<!-- SECTION 6: SECURITY -->
<section class="section" id="sec-security">
  <div class="container">
    <p class="sec-num">06</p>
    <h2 class="sec-heading">Security &amp; Anti-Cheat</h2>
    <div class="sec-heading-bar"></div>
    <table class="tbl">
      <thead><tr><th>Security Feature</th><th>Protection</th></tr></thead>
      <tbody>
        <tr><td>&#10003; Server Timestamp</td><td>Ang oras ay galing sa Google Server — hindi madadaya kahit baguhin ng employee ang oras ng kanyang phone</td></tr>
        <tr><td>&#10003; Live Camera Only</td><td>Hindi makakapag-upload ng lumang photo mula sa gallery — live feed lang ang tinatanggap</td></tr>
        <tr><td>&#10003; 10-Second Expiry</td><td>Kailangang mag-submit agad pagkatapos kumuha ng selfie — hindi pwedeng mag-late submit</td></tr>
        <tr><td>&#10003; Burned-in Timestamp</td><td>Ang timestamp at GPS ay naka-burned sa loob ng larawan — hindi na matatanggal o maedit</td></tr>
        <tr><td>&#10003; GPS Location</td><td>Kinukuha ang real-time GPS — makikita sa admin dashboard kung nasaan talaga ang employee</td></tr>
        <tr><td>&#10003; OpenStreetMap Verify</td><td>Pwedeng i-click ang address sa admin dashboard para i-verify sa mapa kung tama ang location</td></tr>
      </tbody>
    </table>
    <div class="alert warning">
      <strong>&#9888;&#65039; Mga Posibleng Panganib:</strong>
      <ul>
        <li><strong>GPS Spoofing</strong> — pwedeng gumamit ng GPS spoofer app ang tech-savvy employees. Solusyon: Geofencing (advanced feature)</li>
        <li><strong>Virtual Camera</strong> — pwedeng gumamit ng virtual camera app. Kailangan ng advanced technical knowledge</li>
        <li><strong>Ibang tao ang mag-selfie</strong> — pabayaan ang kasama na kumuha ng selfie para sa kanila</li>
      </ul>
      <p style="margin-top:8px;font-style:italic">Ang combination ng server timestamp + live selfie + GPS ay sapat para sa basic attendance monitoring.</p>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <strong>PHOTOLINE</strong> — Field Duty Timesheet System Documentation<br>
  Employee App: <a href="https://bit.ly/plineTimesheet" target="_blank"><strong>bit.ly/plineTimesheet</strong></a>
  &nbsp;|&nbsp;
  Admin: <a href="https://photolinepayroll.github.io/attendance-app/admin.html" target="_blank"><strong>...attendance-app/admin.html</strong></a><br>
  Admin Password: <strong>Photoline.201</strong> &nbsp;|&nbsp; Prepared: May 2026
</footer>
```

- [ ] **Step 3: Verify in browser**

Scroll through all sections. Expected: all 6 sections render, security table shows green check marks, footer shows at bottom.

---

## Task 8: GSAP ScrollTrigger Animations

**Files:**
- Modify: `New Projects/Photoline_Documentation_Animated.html`

- [ ] **Step 1: Replace `// ANIMATIONS GO HERE (Task 8)` with the full animation block**

```js
// ── SECTION HEADING BARS ────────────────────
gsap.utils.toArray('.sec-heading-bar').forEach(bar => {
  gsap.fromTo(bar, { width: 0 }, {
    width: '100%',
    duration: 0.8,
    ease: 'power3.out',
    scrollTrigger: { trigger: bar, start: 'top 85%', once: true }
  });
});

// ── FEATURE CARDS (stagger fade-up) ─────────
gsap.utils.toArray('.feat-grid').forEach(grid => {
  gsap.fromTo(grid.querySelectorAll('.feat-card'),
    { opacity: 0, y: 40 },
    { opacity: 1, y: 0, stagger: 0.1, duration: 0.5, ease: 'power2.out',
      scrollTrigger: { trigger: grid, start: 'top 80%', once: true }
    }
  );
});

// ── STEPS (stagger slide-in) ─────────────────
gsap.utils.toArray('.steps').forEach(steps => {
  gsap.fromTo(steps.querySelectorAll('.step'),
    { opacity: 0, x: -30 },
    { opacity: 1, x: 0, stagger: 0.08, duration: 0.45, ease: 'power2.out',
      scrollTrigger: { trigger: steps, start: 'top 80%', once: true }
    }
  );
});

// ── TABLE ROWS (stagger slide-in) ────────────
gsap.utils.toArray('.tbl tbody tr').forEach((row, i) => {
  gsap.fromTo(row,
    { opacity: 0, x: -20 },
    { opacity: 1, x: 0, duration: 0.4, ease: 'power2.out',
      scrollTrigger: { trigger: row, start: 'top 88%', once: true },
      delay: i * 0.04
    }
  );
});

// ── ALERTS (scale fade) ──────────────────────
gsap.utils.toArray('.alert').forEach(el => {
  gsap.fromTo(el, { opacity: 0, scale: 0.97 },
    { opacity: 1, scale: 1, duration: 0.5, ease: 'power2.out',
      scrollTrigger: { trigger: el, start: 'top 85%', once: true }
    }
  );
});

// ── FAQ ITEMS ────────────────────────────────
gsap.utils.toArray('.faq-item').forEach((item, i) => {
  gsap.fromTo(item, { opacity: 0, y: 16 },
    { opacity: 1, y: 0, duration: 0.35, ease: 'power2.out', delay: i * 0.06,
      scrollTrigger: { trigger: item, start: 'top 88%', once: true }
    }
  );
});

// ── SECTION HEADINGS ─────────────────────────
gsap.utils.toArray('.sec-heading').forEach(el => {
  gsap.fromTo(el, { opacity: 0, y: 20 },
    { opacity: 1, y: 0, duration: 0.5, ease: 'power2.out',
      scrollTrigger: { trigger: el, start: 'top 85%', once: true }
    }
  );
});

// ── 3D TILT ON CARDS ─────────────────────────
if (!('ontouchstart' in window)) {
  document.querySelectorAll('.feat-card').forEach(card => {
    card.addEventListener('mousemove', e => {
      const rect = card.getBoundingClientRect();
      const x = (e.clientX - rect.left) / rect.width  - 0.5;
      const y = (e.clientY - rect.top)  / rect.height - 0.5;
      gsap.to(card, { rotateY: x * 14, rotateX: -y * 14, duration: 0.3,
        transformPerspective: 600, ease: 'power1.out' });
    });
    card.addEventListener('mouseleave', () => {
      gsap.to(card, { rotateY: 0, rotateX: 0, duration: 0.5, ease: 'elastic.out(1, 0.5)' });
    });
  });
}
```

- [ ] **Step 2: Verify animations in browser**

Scroll slowly through the page. Expected:
- Feature cards fade up with stagger when scrolled into view
- Step items slide in from left one-by-one
- Table rows animate in on scroll
- Section heading underlines draw from left
- On desktop: 3D tilt on card hover
- No console errors

---

## Task 9: Mobile Responsive + Polish

**Files:**
- Modify: `New Projects/Photoline_Documentation_Animated.html`

- [ ] **Step 1: Add mobile CSS (at end of `<style>` block)**

```css
/* ── MOBILE ─────────────────────────────────── */
@media (max-width: 768px) {
  .hero-title { font-size: clamp(2.5rem, 12vw, 4rem); }
  .hero-links { grid-template-columns: 1fr; }
  .feat-grid  { grid-template-columns: 1fr; }
  .nav-inner  { padding: 12px 16px; }
  .container  { padding: 0 16px; }
  .section    { padding: 56px 0; }
  .tbl        { font-size: 0.78em; }
  .tbl th, .tbl td { padding: 8px 10px; }
}
@media print {
  #main-nav, #progress-bar, #hero { display: none; }
  body { background: #fff; color: #000; }
  .section { border-top: 1px solid #ccc; padding: 24px 0; }
  .feat-card { border: 1px solid #ccc; }
  .alert { border: 1px solid #ccc; }
}
```

- [ ] **Step 2: Final full-page verification**

Open file in Chrome. Verify:
1. Hero: PHOTOLINE letters animate in, Vanta particles on desktop
2. Nav: goes solid on scroll, progress bar fills, active link highlights
3. Feature cards: stagger in on scroll, 3D tilt on hover (desktop)
4. Steps: slide in from left, green/red colors correct
5. Tables: rows animate in
6. Password box: gold styling correct
7. All Filipino text renders correctly
8. No console errors
9. Narrow window (375px): single column, no horizontal scroll

---

## Self-Review vs Spec

| Spec requirement | Covered in task |
|-----------------|----------------|
| GSAP + ScrollTrigger | Task 1 (CDN), Task 8 (animations) |
| Vanta.js NET background | Task 3 (hero) |
| CSS 3D tilt on cards | Task 8 |
| Scroll progress bar | Task 2 (nav) |
| Active nav highlighting | Task 2 (IntersectionObserver) |
| All 6 content sections | Tasks 5–7 |
| Hero letter-by-letter animation | Task 3 |
| Step items slide-in | Task 8 |
| Table row stagger | Task 8 |
| Alert boxes | Tasks 5–7 |
| FAQ items | Task 6 |
| Password box (gold) | Task 7 |
| Mobile: single column | Task 9 |
| Mobile: no Vanta | Task 3 (isMobile check) |
| Mobile: no 3D tilt | Task 8 (ontouchstart check) |
| Print CSS | Task 9 |
| All content preserved | Tasks 5–7 |
