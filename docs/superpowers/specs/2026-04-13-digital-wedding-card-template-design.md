# Digital Wedding Card Template Design (Hafiz & Nuqi)

## Goal

Deliver a polished single-page digital wedding card template in `index.html` with:

- Soft elegant visual style (light floral slate theme)
- Smooth mobile-first UX
- Scroll-based section animations
- Compact floating music control with reliable autoplay fallback behavior
- Placeholder-friendly content fields for later real data replacement

## Scope

### In Scope

- Single-page structure with clear sections:
  - Hero
  - Countdown
  - Jemputan
  - Butiran Majlis
  - Aturcara
  - Galeri
  - Ucapan
  - Salam Kauk
  - RSVP
- Bottom navigation for quick section jumps
- Soft elegant animation system (fade/slide reveal + subtle interaction polish)
- YouTube IFrame music integration with autoplay-safe fallback
- Persistent compact floating music button at bottom-right
- Placeholder content retained for phone numbers, family names, banking, gallery assets, and QR

### Out of Scope (This Pass)

- Replacing placeholders with final real values
- Adding backend/database
- Migrating to framework
- Analytics integration
- Custom domain setup

## UX Design

### Layout

- Mobile-first, card-based sections stacked vertically for natural scroll behavior
- White translucent cards on slate floral background
- Consistent spacing rhythm and readable typography hierarchy
- Bottom nav acts as lightweight utility layer without interrupting scrolling

### Visual Style

- Palette:
  - `--slate: #7a9aa8`
  - `--slate-dark: #5a7a88`
  - `--card-bg: rgba(255,255,255,0.88)`
- Aesthetic: clean, airy, romantic-modern (no dark-gold theme)
- Card treatment: light blur/glass feel with soft border and shadow

### Motion Design (Soft Elegant)

- Section reveal via IntersectionObserver:
  - Initial: `opacity: 0`, `transform: translateY(18px)`
  - Reveal: `opacity: 1`, `transform: translateY(0)`
  - Timing: approx `700ms`, smooth easing
- Hero text uses gentle staggered entrance
- Buttons/cards use subtle lift feedback on hover/tap
- Accessibility guardrail:
  - If `prefers-reduced-motion: reduce`, transitions are minimized/disabled

## Music System Design (Option 1: Hybrid Autoplay-Safe)

### Behavior Requirements

- Music control is always visible as a compact floating button at bottom-right
- One tap toggles play/pause
- A small ring indicator shows current state:
  - Playing: soft green pulse
  - Paused/loading: neutral gray

### Startup Flow

1. Load YouTube IFrame API and initialize player.
2. Attempt autoplay in browser-safe way.
3. If blocked by policy, keep control visible and ready.
4. On first meaningful user interaction (tap/click/scroll), retry resume/unmute path.
5. Keep user in control with one-tap toggle at all times.

### Fallback Handling

- If API/player is not ready:
  - Button indicates loading/disabled state
- Once ready:
  - Button becomes active without requiring reload
- Fail-safe:
  - UI remains functional even if autoplay never succeeds

## Component Boundaries

### HTML Structure

- Semantic sections (`section`, `header`, `nav`, `footer` where relevant)
- Dedicated floating music control container
- Modal container only for lightweight interactions if needed

### CSS Layers

- Theme variables + global reset/base
- Layout system (containers, cards, grids)
- Component styles (buttons, nav, gallery, timeline)
- Animation utilities (`reveal`, `is-visible`, motion-safe variants)
- Music FAB and indicator styling

### JavaScript Modules (Within Single File)

- Countdown module (target date and live updates)
- Scroll reveal module (observer lifecycle)
- Music module:
  - API boot
  - Player state sync
  - Autoplay retry orchestration
  - Floating control events
- Utility helpers for graceful fallback and state class toggling

## Data Placeholders

Template keeps placeholder values clearly labeled for later replacement:

- WhatsApp number placeholders (Hafiz and Nuqi)
- Parent names in invitation section
- Bank account and account holder
- `images/foto1.jpg` ... `images/foto6.jpg`
- `images/qr.jpg`
- Optional `images/bg.jpg`

## Error Handling

- Missing media assets should not break layout; show graceful placeholder frame
- YouTube load delays should not break page interactions
- Countdown handles invalid/missing target date by showing fallback text
- JS failures in non-critical modules should not block static content readability

## Testing Plan

- Manual mobile-first test on at least one Android browser
- Desktop sanity test (Chrome/Edge)
- Validate:
  - Smooth section reveal behavior
  - Reduced-motion behavior
  - Floating compact music button visibility and toggle state
  - Autoplay fallback path (blocked autoplay scenario)
  - Bottom nav anchor jumps
  - Gallery and modal interactions
  - Countdown correctness

## Success Criteria

- Page feels premium and lightweight on first load
- All sections render correctly with placeholders intact
- Animation feels smooth, not flashy
- Floating music control works as one-tap play/pause with state indicator
- User can proceed to final content replacement without structural code changes