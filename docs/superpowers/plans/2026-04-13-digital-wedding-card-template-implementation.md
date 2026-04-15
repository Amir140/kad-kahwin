# Digital Wedding Card Template Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file, mobile-first `index.html` digital wedding card template with soft elegant animations and a compact floating music control using autoplay-safe behavior.

**Architecture:** Keep all implementation in one static `index.html` file with three clear layers: semantic HTML sections, theme/animation CSS, and modular vanilla JS logic for countdown, reveal effects, and YouTube music control. Preserve placeholders for user data and assets so later content replacement is a text-only update.

**Tech Stack:** HTML5, CSS3, vanilla JavaScript, Google Fonts, YouTube IFrame API.

---

### Task 1: Create Initial Single-File Template Shell

**Files:**
- Create: `index.html`
- Test: `index.html` (manual browser open)

- [ ] **Step 1: Create base HTML skeleton with metadata and font imports**

```html
<!doctype html>
<html lang="ms">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Undangan Majlis | Hafiz & Nuqi</title>
  <meta name="description" content="Kad Kahwin Digital Hafiz & Nuqi">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600;700&family=Great+Vibes&family=Jost:wght@300;400;500;600&display=swap" rel="stylesheet">
</head>
<body>
  <main id="app"></main>
</body>
</html>
```

- [ ] **Step 2: Add semantic section placeholders inside `<main id="app">`**

```html
<main id="app">
  <section id="hero"></section>
  <section id="countdown"></section>
  <section id="jemputan"></section>
  <section id="butiran"></section>
  <section id="aturcara"></section>
  <section id="galeri"></section>
  <section id="ucapan"></section>
  <section id="salam-kauk"></section>
  <section id="rsvp"></section>
</main>
```

- [ ] **Step 3: Open in browser to verify no malformed markup**

Run: `start index.html`  
Expected: page opens without blank-parser errors in DevTools console.

- [ ] **Step 4: Commit initial shell**

```bash
git add index.html
git commit -m "feat: add single-file wedding card template shell"
```

### Task 2: Implement Visual Theme, Layout, and Components

**Files:**
- Modify: `index.html`
- Test: `index.html` (manual responsive check)

- [ ] **Step 1: Add theme variables and global styles in `<style>` block**

```css
:root {
  --slate: #7a9aa8;
  --slate-dark: #5a7a88;
  --card-bg: rgba(255, 255, 255, 0.88);
  --text: #2f3a40;
  --muted: #5f6d74;
  --border: rgba(90, 122, 136, 0.2);
  --shadow: 0 10px 30px rgba(39, 58, 66, 0.14);
}
* { box-sizing: border-box; }
body {
  margin: 0;
  font-family: "Jost", sans-serif;
  color: var(--text);
  background-color: var(--slate);
}
```

- [ ] **Step 2: Add mobile-first layout classes and reusable card styles**

```css
.container { width: min(100% - 1.5rem, 980px); margin-inline: auto; }
.section { padding: 1.25rem 0; }
.card {
  background: var(--card-bg);
  border: 1px solid var(--border);
  border-radius: 22px;
  box-shadow: var(--shadow);
  backdrop-filter: blur(4px);
  padding: 1.1rem;
}
```

- [ ] **Step 3: Build full section markup with placeholders**

```html
<section id="hero" class="section">
  <div class="container">
    <article class="card">
      <p class="bismillah">Bismillahirrahmanirrahim</p>
      <h1>Hafiz &amp; Nuqi</h1>
      <p>29 Ogos 2026 (Sabtu)</p>
      <p>Dewan Serbaguna Bedong, Kedah</p>
    </article>
  </div>
</section>
<!-- Repeat all sections with placeholder content and IDs from spec -->
```

- [ ] **Step 4: Verify responsive behavior at 360px and 768px widths**

Run: manual check using browser responsive mode  
Expected: no horizontal scroll, cards readable, spacing consistent.

- [ ] **Step 5: Commit theme and section structure**

```bash
git add index.html
git commit -m "feat: add floral slate theme and full section placeholders"
```

### Task 3: Add Soft Elegant Animations and Motion Accessibility

**Files:**
- Modify: `index.html`
- Test: `index.html` (manual scroll interaction)

- [ ] **Step 1: Add reveal and interaction animation CSS classes**

```css
.reveal {
  opacity: 0;
  transform: translateY(18px);
  transition: opacity 700ms ease, transform 700ms ease;
}
.reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}
.btn:hover { transform: translateY(-2px); }
```

- [ ] **Step 2: Add reduced-motion override**

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 1ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 1ms !important;
    scroll-behavior: auto !important;
  }
}
```

- [ ] **Step 3: Add IntersectionObserver reveal script**

```javascript
const revealEls = document.querySelectorAll(".reveal");
const revealObserver = new IntersectionObserver((entries, observer) => {
  entries.forEach((entry) => {
    if (!entry.isIntersecting) return;
    entry.target.classList.add("is-visible");
    observer.unobserve(entry.target);
  });
}, { threshold: 0.12 });
revealEls.forEach((el) => revealObserver.observe(el));
```

- [ ] **Step 4: Verify animation behavior and reduced-motion fallback**

Run: manual scroll + DevTools emulate reduced motion  
Expected: smooth fade/slide normally, near-instant transitions when reduced motion enabled.

- [ ] **Step 5: Commit animation system**

```bash
git add index.html
git commit -m "feat: add soft reveal animations with reduced-motion support"
```

### Task 4: Implement Countdown and Utility Interactions

**Files:**
- Modify: `index.html`
- Test: `index.html` (manual + console)

- [ ] **Step 1: Add countdown markup with day/hour/min/sec placeholders**

```html
<div class="countdown-grid">
  <div><span id="days">00</span><small>Hari</small></div>
  <div><span id="hours">00</span><small>Jam</small></div>
  <div><span id="minutes">00</span><small>Minit</small></div>
  <div><span id="seconds">00</span><small>Saat</small></div>
</div>
```

- [ ] **Step 2: Implement countdown timer logic for 2026-08-29 11:00**

```javascript
const targetDate = new Date("2026-08-29T11:00:00+08:00");
function tickCountdown() {
  const diff = targetDate - new Date();
  if (Number.isNaN(targetDate.getTime()) || diff <= 0) return;
  const days = Math.floor(diff / 86400000);
  const hours = Math.floor((diff % 86400000) / 3600000);
  const minutes = Math.floor((diff % 3600000) / 60000);
  const seconds = Math.floor((diff % 60000) / 1000);
  document.getElementById("days").textContent = String(days).padStart(2, "0");
  document.getElementById("hours").textContent = String(hours).padStart(2, "0");
  document.getElementById("minutes").textContent = String(minutes).padStart(2, "0");
  document.getElementById("seconds").textContent = String(seconds).padStart(2, "0");
}
setInterval(tickCountdown, 1000);
tickCountdown();
```

- [ ] **Step 3: Add graceful expired/fallback text behavior**

```javascript
if (diff <= 0) {
  document.querySelector(".countdown-grid").innerHTML = "<p>Majlis sedang berlangsung. Jumpa anda di sana!</p>";
  return;
}
```

- [ ] **Step 4: Verify countdown updates every second and never shows NaN**

Run: manual browser test  
Expected: values decrement correctly; no console errors.

- [ ] **Step 5: Commit countdown feature**

```bash
git add index.html
git commit -m "feat: add live countdown with graceful fallback state"
```

### Task 5: Add Compact Floating Music Control with YouTube Autoplay-Safe Logic

**Files:**
- Modify: `index.html`
- Test: `index.html` (manual autoplay + interaction)

- [ ] **Step 1: Add floating compact music button markup and indicator ring**

```html
<button id="musicFab" class="music-fab" aria-label="Toggle music" title="Toggle music">
  <span class="ring" aria-hidden="true"></span>
  <span class="icon" aria-hidden="true">♪</span>
</button>
<div id="ytPlayer" hidden></div>
```

- [ ] **Step 2: Add music FAB CSS states (loading/playing/paused)**

```css
.music-fab {
  position: fixed;
  right: 1rem;
  bottom: 1rem;
  width: 52px;
  height: 52px;
  border-radius: 999px;
  border: 1px solid var(--border);
}
.music-fab .ring { width: 10px; height: 10px; border-radius: 999px; background: #9aa8ae; }
.music-fab.is-playing .ring { background: #49b67b; animation: pulse 1.8s ease infinite; }
```

- [ ] **Step 3: Add YouTube IFrame API boot and player setup**

```javascript
let ytPlayer;
window.onYouTubeIframeAPIReady = () => {
  ytPlayer = new YT.Player("ytPlayer", {
    videoId: "3k6yn8Yc8CA",
    playerVars: { playsinline: 1, controls: 0, rel: 0 },
    events: { onReady: onPlayerReady, onStateChange: onPlayerStateChange }
  });
};
```

- [ ] **Step 4: Add autoplay-safe flow and first-interaction unmute retry**

```javascript
let hasInteracted = false;
function onPlayerReady() {
  try {
    ytPlayer.mute();
    ytPlayer.playVideo();
  } catch {}
}
["click", "touchstart", "scroll"].forEach((evt) => {
  window.addEventListener(evt, () => {
    if (hasInteracted || !ytPlayer) return;
    hasInteracted = true;
    try { ytPlayer.unMute(); ytPlayer.playVideo(); } catch {}
  }, { once: true, passive: true });
});
```

- [ ] **Step 5: Wire one-tap toggle Play/Pause from floating button**

```javascript
document.getElementById("musicFab").addEventListener("click", () => {
  if (!ytPlayer || typeof ytPlayer.getPlayerState !== "function") return;
  const state = ytPlayer.getPlayerState();
  if (state === YT.PlayerState.PLAYING) ytPlayer.pauseVideo();
  else ytPlayer.playVideo();
});
```

- [ ] **Step 6: Verify blocked-autoplay scenario and manual recovery**

Run: manual test on mobile + desktop  
Expected: button always visible, tap toggles play/pause, ring state reflects player state.

- [ ] **Step 7: Commit music module**

```bash
git add index.html
git commit -m "feat: add compact floating music control with autoplay-safe fallback"
```

### Task 6: Add Bottom Nav, Gallery Lightbox, and Placeholder CTA Integrations

**Files:**
- Modify: `index.html`
- Test: `index.html` (manual interaction)

- [ ] **Step 1: Add fixed bottom nav with anchor links**

```html
<nav class="bottom-nav">
  <a href="#contact">Contact</a>
  <a href="#hero">Music</a>
  <a href="#butiran">Location</a>
  <button type="button" id="calendarBtn">Calendar</button>
</nav>
```

- [ ] **Step 2: Add gallery grid placeholders and lightbox modal markup**

```html
<div class="gallery-grid">
  <img src="images/foto1.jpg" alt="Galeri 1" data-lightbox>
  <img src="images/foto2.jpg" alt="Galeri 2" data-lightbox>
</div>
<dialog id="lightbox">
  <img id="lightboxImage" alt="Pratonton galeri">
  <button id="closeLightbox">Tutup</button>
</dialog>
```

- [ ] **Step 3: Add CTA links with placeholders for WhatsApp and bank details**

```html
<a href="https://wa.me/601XXXXXXXX" target="_blank" rel="noopener">RSVP</a>
<a href="https://wa.me/601XXXXXXXX?text=Assalamualaikum%20..." target="_blank" rel="noopener">Ucapan</a>
<p>No. Akaun: 00-0000000-00 (Placeholder)</p>
```

- [ ] **Step 4: Add lightbox open/close script**

```javascript
const lightbox = document.getElementById("lightbox");
const preview = document.getElementById("lightboxImage");
document.querySelectorAll("[data-lightbox]").forEach((img) => {
  img.addEventListener("click", () => {
    preview.src = img.src;
    lightbox.showModal();
  });
});
document.getElementById("closeLightbox").addEventListener("click", () => lightbox.close());
```

- [ ] **Step 5: Verify nav jumps, gallery preview, and CTA URLs formatting**

Run: manual interaction pass  
Expected: anchors scroll correctly, modal works, URLs stay in `wa.me/601...` format.

- [ ] **Step 6: Commit interactions**

```bash
git add index.html
git commit -m "feat: add bottom navigation, gallery lightbox, and placeholder CTAs"
```

### Task 7: Final Verification and Readiness Pass

**Files:**
- Modify: `index.html` (only if fixes needed)
- Test: `index.html`

- [ ] **Step 1: Run final manual QA checklist**

Run: open `index.html` in desktop + mobile browser  
Expected:
- section order correct
- soft animations smooth
- reduced motion respected
- countdown stable
- music FAB always visible and functional
- no blocking JS errors

- [ ] **Step 2: Perform placeholder integrity check**

Run: search in file for placeholders  
Expected: placeholders remain for numbers, parents, gallery images, QR, account details.

- [ ] **Step 3: Optional cleanup commit (only if fixes made)**

```bash
git add index.html
git commit -m "chore: polish wedding template interactions and accessibility details"
```
