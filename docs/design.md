# Design System

Aesthetic: **"Smoky Blues Club"** — dimly lit stage, warm spotlights cutting through haze, worn leather, vintage microphones.

---

## Color Palette (CSS variables in `global.css`)

```css
:root {
  --color-bg-deep: #0a0a0f; /* Near-black, deep night */
  --color-bg-surface: #141420; /* Card/section backgrounds */
  --color-bg-elevated: #1e1e2e; /* Hover states, nav */
  --color-accent: #c4842d; /* Warm amber/gold — stage spotlight */
  --color-accent-hover: #d4973d; /* Lighter amber on hover */
  --color-text-primary: #e8e4dc; /* Warm off-white */
  --color-text-muted: #8a8698; /* Secondary text */
  --color-border: #2a2a3a; /* Subtle dividers */
}
```

Use alternating section backgrounds (`--color-bg-deep` and `--color-bg-surface`) for visual rhythm.

---

## Typography

- **Headings:** Distinctive serif or display font — e.g., `Playfair Display`, `Cormorant Garamond`, or `DM Serif Display` (Google Fonts, free)
- **Body:** Clean readable font — e.g., `Source Sans 3`, `Libre Franklin`, or `Outfit`
- **Accent/Nav:** Slightly condensed or all-caps treatment for navigation links
- Load via Google Fonts `<link>` in `BaseLayout.astro` head

---

## Animation System

### Button Animations

```css
/* Base button style */
.btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  padding: 0.75rem 2rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  border: 2px solid var(--color-accent);
  color: var(--color-text-primary);
  background: transparent;
  overflow: hidden;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

/* Hover: fill from left */
.btn::before {
  content: "";
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: var(--color-accent);
  transition: left 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  z-index: -1;
}

.btn:hover::before {
  left: 0;
}

.btn:hover {
  color: var(--color-bg-deep);
  box-shadow: 0 0 20px rgba(196, 132, 45, 0.3);
  transform: translateY(-2px);
}

/* Click/active: press down */
.btn:active {
  transform: translateY(1px);
  box-shadow: 0 0 10px rgba(196, 132, 45, 0.2);
}
```

### Available Button Animation Types

- **Fill sweep** — background color sweeps in from left on hover (primary CTA style)
- **Glow pulse** — subtle pulsing box-shadow on important CTAs (e.g., "See us live")
- **Scale lift** — `transform: scale(1.05) translateY(-2px)` on hover
- **Border draw** — border animates in from corners (decorative, secondary actions)
- **Icon slide** — arrow icon slides right on hover for "Learn more" type links
- **Magnetic pull** — button subtly follows cursor on hover (JS, optional, desktop only)

### Scroll-Triggered Section Animations

```css
/* Fade-up on scroll enter */
.reveal {
  opacity: 0;
  transform: translateY(30px);
  transition:
    opacity 0.8s ease,
    transform 0.8s ease;
}

.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Staggered children */
.reveal-stagger > * {
  opacity: 0;
  transform: translateY(20px);
  transition:
    opacity 0.6s ease,
    transform 0.6s ease;
}

.reveal-stagger.visible > *:nth-child(1) {
  transition-delay: 0.1s;
}
.reveal-stagger.visible > *:nth-child(2) {
  transition-delay: 0.2s;
}
.reveal-stagger.visible > *:nth-child(3) {
  transition-delay: 0.3s;
}
/* ... pattern continues */

.reveal-stagger.visible > * {
  opacity: 1;
  transform: translateY(0);
}
```

### Intersection Observer for Animations

```javascript
// Place in <script> tag in BaseLayout.astro
const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add("visible");
      }
    });
  },
  { threshold: 0.1 },
);

document.querySelectorAll(".reveal, .reveal-stagger").forEach((el) => {
  observer.observe(el);
});
```

### Card Hover Animations

- **MemberCards:** lift + accent border glow + bio text fade-in
- **DiscographyCards:** subtle tilt (CSS perspective) + shadow deepen
- **ShowCards:** left-border accent grow + slight translate-x

---

## Visual Effects

- **Grain/noise texture** — subtle overlay on dark backgrounds (CSS pseudo-element with SVG noise)
- **Warm gradient overlays** — on hero images (from transparent to deep bg color)
- **Hover micro-interactions** — cards lift with subtle shadow, accent color border glow
- **Scroll-triggered fade-in** — for content sections (Intersection Observer, vanilla JS)
