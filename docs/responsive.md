# Responsive Design

Philosophy: **Mobile-First** — write CSS for mobile, then add complexity for larger screens with `md:` and `lg:` Tailwind prefixes.

---

## Breakpoints (Tailwind defaults)

| Breakpoint | Width   | Target           |
| ---------- | ------- | ---------------- |
| Default    | 0-639px | Mobile phones    |
| `sm`       | 640px+  | Large phones     |
| `md`       | 768px+  | Tablets          |
| `lg`       | 1024px+ | Laptops/desktops |
| `xl`       | 1280px+ | Large screens    |

---

## Section-Specific Responsive Rules

### Navbar

- **Mobile:** Logo left, hamburger right. Drawer menu with anchor links + socials
- **Tablet+:** Full horizontal nav — logo | links | socials

### Home / Hero

- **Mobile:** Band name smaller font, tagline may hide. CTA buttons stack vertically
- **Tablet+:** Full hero text, side-by-side CTAs

### Media

- **All sizes:** Spotify/YouTube embeds full width, height adjusts
- Ensure YouTube wrapper uses `aspect-ratio: 16/9` for responsiveness

### Discography

- **Mobile:** Album cards 1 column, full-width
- **Tablet:** 2 columns
- **Desktop:** 3 columns

### Tour

- **Mobile:** ShowCards stack full-width, compact date format
- **Tablet+:** Cards have more horizontal space, full date display

### About

- **Mobile:** MemberCards 1 column, stacked vertically
- **Tablet:** 2 columns
- **Desktop:** 3-4 columns depending on member count

### Contact

- **Mobile:** Newsletter input + Join button stack vertically. Email button full-width
- **Tablet+:** Newsletter input + Join button inline. Content centered

---

## Scroll-Snap on Mobile

### Critical Considerations

- Use `min-height: 100dvh` (dynamic viewport height) to account for mobile browser chrome (Safari URL bar, Chrome bottom bar)
- Use `min-height: 100vh` (not `height: 100vh`) so content isn't clipped on overflow
- For sections that might overflow on small screens (About with many members, Tour with many shows): allow natural scroll within the section before snapping
- Consider `scroll-snap-type: y proximity` instead of `mandatory` on mobile — prevents trapping users in overflowing sections

### Testing Checklist

- iPhone Safari (dynamic URL bar behavior)
- Android Chrome (bottom bar behavior)
- Tablet landscape + portrait
- Real devices, not just browser devtools

---

## Touch Interactions

- All hover effects must have tap equivalents on touch devices
- Use `@media (hover: hover)` to scope hover-only effects
- Buttons and links: **minimum 44x44px touch target** (WCAG requirement)
- Hamburger menu: slide-in from right, tap outside to close
