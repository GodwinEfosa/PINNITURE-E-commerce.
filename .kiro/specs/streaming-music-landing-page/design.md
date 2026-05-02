# Design Document: Streaming Music Landing Page

## Overview

This document describes the technical design for a single-file HTML streaming music landing page. The page will be a self-contained promotional website showcasing a music streaming platform with an immersive dark aesthetic, kinetic animations, and interactive elements—all without requiring authentication.

## Architecture

### Single-File Structure

The landing page will be implemented as a single HTML file with the following structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WAVR - Feel Every Beat</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">
  <style>
    /* All CSS styles embedded here */
  </style>
</head>
<body>
  <!-- All HTML markup here -->
  <script>
    /* Minimal JavaScript for interactivity */
  </script>
</body>
</html>
```

**Rationale**: Single-file architecture ensures easy deployment, no build process, and simple distribution. All assets (CSS, JS) are embedded directly.

## Design System

### Color Palette

CSS custom properties will define the color system:

```css
:root {
  /* Backgrounds */
  --bg-primary: #0a0a0a;      /* Deep black */
  --bg-secondary: #111111;     /* Dark gray */
  --bg-card: #1a1a1a;          /* Card background */
  
  /* Accent */
  --accent: #00f0ff;           /* Electric cyan */
  --accent-glow: rgba(0, 240, 255, 0.3);
  
  /* Text */
  --text-primary: #ffffff;
  --text-muted: #888888;
  
  /* Borders */
  --border: rgba(255, 255, 255, 0.08);
}
```

**Rationale**: CSS custom properties provide a maintainable, consistent design system. Electric cyan (#00f0ff) provides high contrast against dark backgrounds and conveys energy.

### Typography

```css
:root {
  --font-display: 'Bebas Neue', sans-serif;
  --font-body: 'DM Sans', sans-serif;
}
```

- **Display font (Bebas Neue)**: Heavy, condensed font for headlines and impact
- **Body font (DM Sans)**: Clean, readable sans-serif for body text and UI elements

**Rationale**: Bebas Neue provides bold visual impact for hero headlines. DM Sans offers excellent readability and modern aesthetic for UI elements.

## Component Design

### 1. Hero Section

**Layout**:
- Full viewport height (100vh)
- Centered content with flexbox
- Vertical stacking: headline → tagline → CTA → visualizer

**HTML Structure**:
```html
<section class="hero">
  <div class="hero-content">
    <h1 class="hero-title">Feel Every Beat.</h1>
    <p class="hero-subtitle">Your soundtrack. Everywhere.</p>
    <button class="cta-button">Start Listening</button>
  </div>
  <div class="visualizer">
    <div class="bar"></div>
    <!-- Repeat 20-30 times -->
  </div>
</section>
```

**Animations**:
- Headline: fade-in + slide-up (delay: 0.2s)
- Subtitle: fade-in + slide-up (delay: 0.4s)
- CTA button: fade-in + slide-up (delay: 0.6s)
- Visualizer bars: staggered height animations

```css
@keyframes fadeSlideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.hero-title {
  animation: fadeSlideUp 0.8s ease-out 0.2s both;
}
```

**Rationale**: Full-height hero creates immediate impact. Staggered animations guide user attention from headline to CTA.

### 2. Animated Music Visualizer

**Structure**:
- 25 vertical bars (divs with class "bar")
- Horizontal flexbox layout with gap
- Each bar has unique animation delay

**CSS Implementation**:
```css
.visualizer {
  display: flex;
  align-items: flex-end;
  gap: 4px;
  height: 100px;
}

.bar {
  width: 8px;
  background: var(--accent);
  border-radius: 4px 4px 0 0;
  animation: barPulse 1.2s ease-in-out infinite;
}

.bar:nth-child(1) { animation-delay: 0s; opacity: 0.7; }
.bar:nth-child(2) { animation-delay: 0.1s; opacity: 0.8; }
/* ... continue pattern ... */

@keyframes barPulse {
  0%, 100% { height: 20px; }
  50% { height: 80px; }
}
```

**Rationale**: Pure CSS animation avoids JavaScript overhead. Staggered delays create organic, music-like movement. Varying opacity adds visual depth.

### 3. CTA Button

**Design**:
- Pill shape (border-radius: 50px)
- Electric cyan background
- Pulsing glow effect
- Hover state with scale transform

**CSS Implementation**:
```css
.cta-button {
  background: var(--accent);
  color: var(--bg-primary);
  padding: 16px 48px;
  border-radius: 50px;
  border: none;
  font-weight: 700;
  font-size: 18px;
  cursor: pointer;
  box-shadow: 0 0 20px var(--accent-glow);
  animation: glowPulse 2s ease-in-out infinite;
  transition: transform 0.2s ease;
}

.cta-button:hover {
  transform: scale(1.05);
}

@keyframes glowPulse {
  0%, 100% { box-shadow: 0 0 20px var(--accent-glow); }
  50% { box-shadow: 0 0 40px var(--accent-glow); }
}
```

**Rationale**: Pulsing glow draws attention. Hover feedback confirms interactivity. High contrast ensures visibility.

### 4. Featured Tracks Section

**Layout**:
- Section title: "Trending Now"
- Horizontal scrollable container
- 5 track cards with overflow-x: auto

**Track Card Structure**:
```html
<div class="track-card">
  <div class="album-art"></div>
  <div class="track-info">
    <h3 class="track-name">Midnight Drive</h3>
    <p class="artist-name">Luna Wave</p>
    <span class="duration">3:42</span>
  </div>
  <div class="play-overlay">
    <span class="play-icon">▶</span>
  </div>
</div>
```

**Album Art Placeholder**:
```css
.album-art {
  width: 100%;
  aspect-ratio: 1;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 8px;
}
```

**Hover Interaction**:
```css
.track-card {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.track-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.5);
}

.play-overlay {
  opacity: 0;
  transition: opacity 0.3s ease;
}

.track-card:hover .play-overlay {
  opacity: 1;
}
```

**Rationale**: Horizontal scroll allows more content without vertical space. Gradient placeholders add visual interest. Hover effects provide clear interaction feedback.

### 5. Mood and Genre Picker

**Layout**:
- Section title: "What's Your Vibe?"
- Grid of 6 pill buttons (3 columns on mobile, 6 on desktop)
- Background decorative waveform

**Pill Button Structure**:
```html
<button class="mood-pill">Chill</button>
<button class="mood-pill">Hype</button>
<button class="mood-pill">Focus</button>
<button class="mood-pill">Afrobeats</button>
<button class="mood-pill">Late Night</button>
<button class="mood-pill">Workout</button>
```

**CSS Implementation**:
```css
.mood-pill {
  padding: 12px 32px;
  border: 2px solid var(--border);
  border-radius: 50px;
  background: transparent;
  color: var(--text-primary);
  cursor: pointer;
  transition: all 0.3s ease;
}

.mood-pill:hover {
  background: var(--accent);
  color: var(--bg-primary);
  border-color: var(--accent);
  transform: scale(1.05);
}
```

**Background Waveform**:
```css
.mood-section::before {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  background: url('data:image/svg+xml,...'); /* Inline SVG waveform */
  opacity: 0.05;
  animation: waveMove 10s linear infinite;
}

@keyframes waveMove {
  0% { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}
```

**Rationale**: Pill buttons are trendy and touch-friendly. Hover fill provides clear feedback. Subtle animated background adds depth without distraction.

### 6. Always Playing Feature Highlight

**Layout**:
- Two-column split (text left, phone mockup right)
- Responsive: stacks vertically on mobile

**HTML Structure**:
```html
<section class="feature-highlight">
  <div class="feature-text">
    <h2>Your soundtrack. Everywhere.</h2>
    <p>Seamless playback across all your devices.</p>
  </div>
  <div class="phone-mockup">
    <div class="phone-screen">
      <div class="now-playing">
        <div class="vinyl-disc"></div>
        <h3 class="np-track">Electric Dreams</h3>
        <p class="np-artist">Neon Pulse</p>
        <div class="progress-bar">
          <div class="progress-fill"></div>
        </div>
        <div class="controls">
          <span>⏮</span>
          <span>▶</span>
          <span>⏭</span>
        </div>
      </div>
    </div>
  </div>
</section>
```

**Phone Mockup CSS**:
```css
.phone-mockup {
  width: 300px;
  height: 600px;
  border: 12px solid #333;
  border-radius: 40px;
  background: var(--bg-primary);
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

.phone-screen {
  width: 100%;
  height: 100%;
  border-radius: 28px;
  overflow: hidden;
  background: var(--bg-secondary);
}
```

**Vinyl Disc Animation**:
```css
.vinyl-disc {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background: radial-gradient(circle, #333 30%, #111 70%);
  animation: spin 4s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
```

**Progress Bar Animation**:
```css
.progress-fill {
  height: 100%;
  background: var(--accent);
  animation: progressGrow 8s ease-in-out infinite;
}

@keyframes progressGrow {
  0% { width: 0%; }
  50% { width: 60%; }
  100% { width: 0%; }
}
```

**Rationale**: Split layout showcases feature with visual proof. Spinning vinyl and animated progress bar create sense of active playback. Phone mockup demonstrates mobile experience.

### 7. Floating Music Notes

**Implementation**:
```html
<div class="floating-notes">
  <span class="note">♪</span>
  <span class="note">♪</span>
  <span class="note">♪</span>
  <span class="note">♪</span>
  <span class="note">♪</span>
</div>
```

**CSS Animation**:
```css
.note {
  position: absolute;
  font-size: 24px;
  color: var(--accent);
  opacity: 0;
  animation: floatUp 6s ease-in infinite;
}

.note:nth-child(1) { left: 10%; animation-delay: 0s; }
.note:nth-child(2) { left: 30%; animation-delay: 1.2s; }
.note:nth-child(3) { left: 50%; animation-delay: 2.4s; }
.note:nth-child(4) { left: 70%; animation-delay: 3.6s; }
.note:nth-child(5) { left: 90%; animation-delay: 4.8s; }

@keyframes floatUp {
  0% {
    bottom: 0;
    opacity: 0;
  }
  20% {
    opacity: 0.6;
  }
  100% {
    bottom: 100vh;
    opacity: 0;
  }
}
```

**Rationale**: Floating notes add whimsy and reinforce music theme. Staggered timing prevents visual clutter. Subtle opacity prevents distraction from main content.

### 8. Footer

**Layout**:
- Three-column grid: logo | navigation | tagline
- Responsive: stacks vertically on mobile

**HTML Structure**:
```html
<footer class="footer">
  <div class="footer-logo">WAVR</div>
  <nav class="footer-nav">
    <a href="#discover">Discover</a>
    <a href="#artists">Artists</a>
    <a href="#playlists">Playlists</a>
    <a href="#about">About</a>
  </nav>
  <p class="footer-tagline">Music that moves with you.</p>
</footer>
```

**CSS Styling**:
```css
.footer {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  gap: 32px;
  padding: 48px 24px;
  background: var(--bg-secondary);
  border-top: 1px solid var(--border);
}

.footer-nav {
  display: flex;
  gap: 24px;
  justify-content: center;
}

.footer-nav a {
  color: var(--text-muted);
  text-decoration: none;
  transition: color 0.2s ease;
}

.footer-nav a:hover {
  color: var(--accent);
}
```

**Rationale**: Grid layout provides balanced structure. Minimal design keeps focus on main content. Hover effects maintain interactivity consistency.

## Responsive Design Strategy

### Breakpoints

```css
/* Mobile-first base styles */
/* 375px - 768px */

@media (min-width: 768px) {
  /* Tablet styles */
}

@media (min-width: 1024px) {
  /* Desktop styles */
}
```

### Key Responsive Adjustments

**Hero Section**:
- Mobile: font-size: 48px, padding: 24px
- Desktop: font-size: 96px, padding: 48px

**Track Cards**:
- Mobile: width: 200px, horizontal scroll
- Desktop: width: 250px, horizontal scroll

**Mood Pills**:
- Mobile: 2 columns grid
- Tablet: 3 columns grid
- Desktop: 6 columns grid (single row)

**Feature Highlight**:
- Mobile: vertical stack
- Desktop: two-column split (50/50)

**Footer**:
- Mobile: single column stack
- Desktop: three-column grid

**Rationale**: Mobile-first approach ensures core experience works on smallest screens. Progressive enhancement adds layout complexity for larger viewports.

## Performance Considerations

### CSS Animation Optimization

All animations use `transform` and `opacity` properties exclusively to ensure GPU acceleration:

```css
/* Good - GPU accelerated */
.element {
  transform: translateY(-8px);
  opacity: 0.8;
}

/* Avoid - triggers layout recalculation */
.element {
  top: -8px;
  height: 100px;
}
```

### Animation Timing

- Use `ease-out` for entrance animations (elements appearing)
- Use `ease-in-out` for continuous loops (visualizer, glow)
- Use `linear` for constant motion (vinyl spin, waveform)

**Rationale**: Proper timing functions create natural, organic motion. GPU-accelerated properties prevent jank and ensure smooth 60fps animations.

## Accessibility Considerations

While not explicitly required, basic accessibility practices will be implemented:

- Semantic HTML5 elements (`<section>`, `<nav>`, `<footer>`)
- Sufficient color contrast (cyan on black exceeds WCAG AA)
- Focus states for interactive elements
- Alt text for decorative elements (empty alt="" for music notes)

**Rationale**: Basic accessibility ensures broader usability without significant additional effort.

## Implementation Notes

### No JavaScript Required for Animations

All animations (visualizer, vinyl spin, progress bar, floating notes, glow effects) are implemented with pure CSS. JavaScript is only needed for:
- Potential smooth scroll polyfill (if needed for older browsers)
- Future interactivity (if CTA button needs to trigger actions)

**Rationale**: CSS animations are more performant, require less code, and work without JavaScript enabled.

### Font Loading Strategy

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">
```

- Preconnect to Google Fonts for faster DNS resolution
- Use `display=swap` to prevent invisible text during font load

**Rationale**: Optimized font loading prevents layout shift and ensures text remains visible during load.

## File Structure Summary

```
index.html
├── <head>
│   ├── Meta tags
│   ├── Google Fonts link
│   └── <style> (all CSS)
└── <body>
    ├── Hero Section
    ├── Featured Tracks Section
    ├── Mood Picker Section
    ├── Feature Highlight Section
    ├── Floating Notes
    ├── Footer
    └── <script> (minimal JS if needed)
```

**Total file size estimate**: ~15-20KB (uncompressed)

## Design Decisions Summary

1. **Single-file architecture**: Simplifies deployment and distribution
2. **Electric cyan accent**: High contrast, energetic, modern
3. **Pure CSS animations**: Better performance, less code complexity
4. **Mobile-first responsive**: Ensures core experience on all devices
5. **Dark aesthetic**: Conveys late-night music listening experience
6. **Gradient placeholders**: Visual interest without external images
7. **Staggered animations**: Creates organic, non-robotic feel
8. **Horizontal scroll for tracks**: Maximizes content without vertical space
9. **Phone mockup with animations**: Demonstrates active playback experience
10. **Minimal footer**: Keeps focus on main content and CTA

## Conclusion

This design creates an immersive, kinetic landing page that conveys the energy and aesthetic of a modern music streaming platform. The single-file architecture ensures easy deployment, while pure CSS animations provide smooth, performant visual effects. The dark aesthetic with electric cyan accents creates a distinctive brand identity that stands out in the music streaming space.
