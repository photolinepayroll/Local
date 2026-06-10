# Photoline Animated Documentation — Design Spec
**Date:** 2026-06-06  
**Status:** Approved  
**Output file:** `d:\GJA-CODING\New Projects\Photoline_Documentation_Animated.html`

---

## 1. Goal

Transform the existing static `Photoline_Documentation.html` into a visually stunning, single-file animated HTML documentation page using GSAP + Vanta.js (Option A). All original content (Filipino/English mixed) is preserved exactly. No content is added or removed.

---

## 2. Tech Stack

| Library | Version | CDN | Purpose |
|---------|---------|-----|---------|
| GSAP | 3.12 | cdnjs | All animations, timelines |
| ScrollTrigger | 3.12 | cdnjs | Scroll-driven reveals |
| Three.js | r134 | cdnjs | Vanta.js dependency |
| Vanta.js NET | 0.5.24 | cdnjs | Animated particle network background |
| Inter | — | Google Fonts | Typography |

**No build step.** Single `.html` file, open directly in browser.

---

## 3. Visual Design

### Color Tokens
```
--bg:       #060B18   // Deep navy — page base
--card:     #0D1B2E   // Card and step backgrounds
--blue:     #2563EB   // Primary — nav, headings, badges
--cyan:     #00C8FF   // Accent — glows, underlines, highlights
--green:    #10B981   // Log In steps, success alerts
--red:      #EF4444   // Log Out steps, warnings
--gold:     #F59E0B   // Alerts, password box, TOC
--white:    #F8FAFC   // Body text
--gray:     #94A3B8   // Secondary text
--border:   rgba(37,99,235,0.25)  // Card borders
```

### Typography
- Font: Inter (Google Fonts), weights 400/500/600/700/800/900
- Hero title: 900 weight, letter-spacing -2px, 56–72px
- Section headings: 800 weight, 22px
- Body: 400/500, 14px, line-height 1.7

---

## 4. Layout Structure

```
[Sticky Top Nav]        — Logo left, section links right, scroll progress bar
[Hero Section]          — Full-viewport height, Vanta NET background
[Section 1: Overview]   — Feature cards grid (2×3)
[Section 2: Employee]   — Log In steps (green) + Log Out steps (red)
[Section 3: Admin]      — Feature list + summary table + password box
[Section 4: Photo]      — Info table + timer alert
[Section 5: Sheets]     — Column table + Drive info alert
[Section 6: Security]   — Security table + warning alert
[Footer]
```

---

## 5. Animation Plan

### Hero
- GSAP timeline on page load (no scroll needed)
- "PHOTOLINE" letters split and blur-in staggered (0.05s per letter)
- Subtitle fades up 0.4s after title
- Two link cards slide up with stagger
- Vanta NET: blue (#2563EB) lines on dark (#060B18) background, speed 0.5

### Sticky Nav
- Transparent on hero, solid `#060B18` + border on scroll past hero
- Scroll progress bar (cyan, full width, 3px, fixed top)
- Active section link highlighted in cyan (IntersectionObserver)

### Feature Cards (Overview — 6 cards)
- ScrollTrigger: stagger 0.12s, `from: { opacity:0, y:40 }`
- On hover: CSS 3D tilt via JS mousemove (`rotateX` ±8deg, `rotateY` ±8deg)
- On hover: cyan glow border `box-shadow: 0 0 20px rgba(0,200,255,0.3)`

### Step Items (Employee Guide)
- Each step reveals `from: { opacity:0, x:-30 }` staggered 0.1s
- Step number circle: CSS `clip-path` animation — fills with color on reveal
- Green steps for Log In, red steps for Log Out

### Admin Feature Items
- ScrollTrigger zoom-in `from: { opacity:0, scale:0.92 }` staggered 0.1s
- Glow border pulses once on entry (GSAP yoyo)

### Tables
- Each `<tr>` slides in `from: { opacity:0, x:-20 }` staggered 0.06s
- First column cells glow cyan on entry

### Alert Boxes
- Fade + slight scale from `scale:0.97` to `1`

### Section Headings
- Underline draws from left (CSS scaleX 0→1 with GSAP)

---

## 6. Content Preservation

All original content is preserved verbatim:
- All Tagalog/Filipino text unchanged
- All links (bit.ly/plineTimesheet, admin.html) unchanged
- Admin password displayed as-is (doc requirement)
- All tables, steps, FAQ items, alerts reproduced exactly
- Print button retained (no-print class)

---

## 7. Responsive Behavior

- Mobile (≤768px): single column for cards and steps, nav links horizontally scrollable (overflow-x: auto, no hamburger)
- Vanta NET disabled on mobile to save performance (replaced by static gradient)
- 3D tilt effect disabled on touch devices

---

## 8. Out of Scope

- No backend / server changes
- No Google Sheets integration changes
- No new content added
- No print layout redesign (existing print CSS retained)
