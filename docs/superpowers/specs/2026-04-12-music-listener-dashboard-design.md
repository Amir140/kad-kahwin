# Music listener dashboard — design spec

**Date:** 2026-04-12  
**Status:** Approved by stakeholder (chat)  
**Program context:** Project 1 of 3 (`music` → `security landing` → `settings + dark mode`). This document defines the listener dashboard; the shared repo layout is recorded here for implementation alignment.

## 1. Purpose and scope

### 1.1 Goal

Ship a **dark-first** listener **home** experience that:

- Gets users **back into playback quickly** (continue listening, clear affordances to library).
- Surfaces **personalized discovery** via horizontal content rails.
- Keeps a **full mini player** visible at all times with believable (mocked) transport state.

### 1.2 Non-goals

- Real audio streaming, DRM, catalog APIs, or auth backends (fixtures and local UI state only).
- Social feeds, live audio, podcasts, or upload flows.
- Full account/settings product (covered by the separate settings project); this screen may include only lightweight header affordances (e.g. avatar) without implementing the settings surface.

## 2. Repository and application shape

### 2.1 Decision

Use **one Next.js (TypeScript) application** with **multiple routes**, grouped so marketing vs app shells stay isolated:

- **Listener dashboard (this spec):** primary implementation target for phase 1, e.g. route group `(app)` under `/music` (exact path to be finalized in implementation plan; default assumption `/music`).
- **AI security landing (phase 2):** route group `(marketing)` at `/`.
- **Settings + dark mode (phase 3):** dedicated route e.g. `/settings`.

### 2.2 Rationale

Single dev server, shared **design tokens** (CSS variables) and primitives, and a natural fit for the marketing landing (SSR/SEO) alongside client-heavy app chrome for music.

### 2.3 Shared foundations

- **Tokens:** dark charcoal surfaces, high-contrast text, one **electric accent** (default: violet; implementation may swap after visual QA).
- **Typography:** geometric sans for UI; optional display weight for section titles.
- **Motion:** respect `prefers-reduced-motion`; carousel scroll remains usable with keyboard.

## 3. Information architecture

### 3.1 Shell layout

| Region | Responsibility |
|--------|----------------|
| Left nav rail | Home, Search, Library, “Create playlist” CTA (non-persisted or stub). |
| Main column | Vertically stacked **carousel sections**; main scroll. |
| Bottom mini player | Artwork, title/artist, like, play/pause, skip, scrubber, volume, queue/device entry (stubs acceptable). |

### 3.2 Responsive behavior

- **Wide:** expanded nav labels.
- **Narrow:** collapse nav to **icon-only** with tooltips; ensure main content bottom padding clears the mini player.

### 3.3 Home content order (top to bottom)

1. **Continue listening** — large cards, prominent artwork.
2. **Personalized / “Made for you”** — horizontal rail.
3. **Quick access** — Liked songs, Recently played (compact cards or pills).
4. **Discovery** — genre/mood rails (mock categories).
5. **New releases** — row of mocked albums/singles.

Each section: title row with optional **“See all”** (stub navigation or disabled state until a later scope iteration).

## 4. Components (conceptual)

| Component | Notes |
|-----------|--------|
| `AppShell` | Composes nav + main + player; enforces min-height and overflow on main. |
| `CarouselSection` | Title, optional subtitle, “See all”, horizontally scrolling track list; keyboard focusable. |
| `TrackCard` / `PlaylistCard` | Consistent aspect ratio, hover/focus, skeleton loading state for simulated fetch. |
| `MiniPlayer` | Fixed height docked to bottom; does not obscure primary CTAs; `aria-label` on icon-only controls. |

## 5. Data and state

- **Content:** static JSON or in-repo fixtures (albums, playlists, tracks) — no network requirement for phase 1.
- **Playback UI state:** **Zustand** (preferred) or React Context for `nowPlaying`, `isPlaying`, and a **simulated progress** tick so the scrubber moves; skip/seek update local time only.

## 6. Accessibility and quality bar

- WCAG-oriented contrast on dark backgrounds for body and interactive elements.
- Visible **focus rings** on all interactive controls.
- Carousels: keyboard operable; avoid trap-focus issues inside horizontal scroll.
- Screen reader labels for icon buttons (play, skip, like, volume, queue).

## 7. Testing expectations (implementation phase)

- **Unit:** state store actions (play/pause, select track, seek simulation).
- **Component:** carousel renders with fixtures; mini player reflects store.
- **Smoke:** route loads without runtime errors in dev build.

## 8. Out of scope reminders

- Projects 2 and 3 are specified in separate design docs after this phase’s implementation plan is executed and reviewed.

## 9. Open items (none blocking spec approval)

- Final accent color token name and value (default violet).
- Exact route path string (`/music` vs `/app`) — pick one at implementation planning for consistency with marketing `/`.
