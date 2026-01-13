# Prompt Template: Premium Glass UI with 3D Floating Icons

Use this prompt (or adapt it) to instruct an AI to create premium, macOS-style glass interfaces with floating 3D icons similar to CleanMyMac.

---

## The Prompt

```
I want you to build a premium, polished web interface with the visual quality of macOS native apps like CleanMyMac. The interface should feature floating 3D icons with a clay/ceramic material look, glass morphism panels, and smooth animations.

## VISUAL REFERENCE

The aesthetic I'm aiming for is:
- Premium macOS utility app style (like CleanMyMac, Raycast, Linear)
- Icons that look like polished clay or ceramic, NOT plastic
- Soft, diffused highlights and shadows
- Frosted glass panels with blur effects
- Subtle floating animations
- Monochromatic color theming

## TECHNICAL REQUIREMENTS

Build this using:
- Pure HTML5, CSS3, and vanilla JavaScript
- No external libraries or frameworks
- No build tools required - single HTML files that work in browser
- Must work on modern browsers (Chrome, Safari, Firefox, Edge)

## THE 3D ICON SYSTEM

Create floating 3D icons using a multi-layer CSS stack. Each icon needs these layers (from back to front):

### Layer 1: Ground Shadow (translateZ: -50 to -60px)
- Elliptical radial gradient
- rgba(dark-theme-color, 0.5) fading to transparent
- Animates in sync with floating (smaller when icon rises, larger when drops)
- Creates grounding effect

### Layer 2: Base Shape (translateZ: -18 to -25px)
- The main colored shape
- Linear gradient from light (top-left) to dark (bottom-right)
- Use 6-8 gradient color stops for smooth transitions
- Angle: 145-160 degrees
- External box-shadows for drop shadow AND ambient glow
- This is the "back" of the 3D object

### Layer 3: Depth/Inner Shadow Layer (translateZ: -10px)
- Same shape as base
- Only contains inner shadows (inset box-shadows)
- Creates the "clay" material feel
- Shadows needed:
  - inset 0 5px 10px rgba(255,255,255,0.4) - top inner highlight
  - inset 0 -8px 20px rgba(dark-color, 0.3) - bottom inner shadow
  - inset 5px 0 10px rgba(255,255,255,0.12) - side rim highlight

### Layer 4: Glaze Overlay (translateZ: 10-12px)
- Slightly smaller than base (inset: 8-12px)
- Subtle white gradient overlay
- Creates "glazed ceramic" sheen
- Gradient: white 0.35 opacity → white 0.12 → white 0.03 → transparent → white 0.05

### Layer 5: Top Reflection (translateZ: 14-18px)
- Crescent-shaped highlight at the top
- White gradient fading downward
- CRITICAL: Use CSS mask-image to contain within the shape bounds
- mask-image: radial-gradient(ellipse 100% 120% at 50% 0%, black 55%, transparent 100%)
- Opacity around 0.9

### Layer 6: Symbol/Icon (translateZ: 25-32px)
- The actual icon (lightning bolt, folder, shield, etc.)
- Use the lightest color from your palette
- Add drop-shadow filter for depth
- Can be SVG or CSS shapes

### Required CSS Properties:
```css
.icon-container {
  perspective: 1000-1200px;
}

.icon-wrapper {
  transform-style: preserve-3d;
}

.each-layer {
  position: absolute;
  transform: translateZ(Xpx);
}
```

## THE CLAY/CERAMIC MATERIAL

The key to the "clay" look (NOT plastic) is the combination of:

1. **Multi-stop gradients** - Use 6-8 color stops, not just 2-3
2. **Inner shadows** - Multiple inset box-shadows create depth
3. **Soft outer shadows** - Large blur radius (40-60px)
4. **Ambient glow** - Colored shadow with 0 offset, large spread
5. **Contained reflections** - Highlights don't bleed outside shape

Example box-shadow for base layer:
```css
box-shadow:
  /* External shadows */
  0 28px 55px rgba(dark-color, 0.55),    /* Main drop shadow */
  0 14px 28px rgba(dark-color, 0.35),    /* Secondary shadow */
  0 0 70px rgba(theme-color, 0.25),       /* Ambient glow */
  /* Internal shadows */
  inset 0 5px 10px rgba(255,255,255,0.4),
  inset 0 -8px 20px rgba(dark-color, 0.3),
  inset 5px 0 10px rgba(255,255,255,0.12);
```

## FLOATING ANIMATION

Create a gentle floating animation that combines:
- Vertical movement (translateY): -8px to -22px range
- Subtle rotation (rotateX/rotateY): 3-7 degrees max
- Duration: 6-8 seconds
- Timing: ease-in-out
- Use 5 keyframe points (0%, 20%, 40%, 60%, 80%, 100%) for organic motion

The ground shadow must animate inversely:
- When icon rises → shadow scales smaller (0.85) and fades (opacity 0.4)
- When icon drops → shadow scales larger (1.0) and darkens (opacity 0.65)

## MOUSE PARALLAX

Add interactive parallax that tilts the icon based on mouse position:

```javascript
// Normalize mouse position to -0.5 to 0.5 range
const x = (mouseX - rect.left) / rect.width - 0.5;
const y = (mouseY - rect.top) / rect.height - 0.5;

// Use interpolation for smoothness (lerp)
currentX += (targetX - currentX) * 0.07;
currentY += (targetY - currentY) * 0.07;

// Apply rotation (invert Y for natural feel)
const rotateX = currentY * -14;  // Max 14 degrees
const rotateY = currentX * 14;
```

## GLASS MORPHISM PANELS

For frosted glass panels:
```css
.glass-panel {
  backdrop-filter: blur(40-60px) saturate(180%);
  -webkit-backdrop-filter: blur(40-60px) saturate(180%);
  background: rgba(255, 255, 255, 0.04-0.08);
  border: 1px solid rgba(255, 255, 255, 0.06-0.1);
  border-radius: 20-28px;
  box-shadow:
    0 40px 80px rgba(0,0,0,0.35),
    inset 0 1px 0 rgba(255,255,255,0.08);
}
```

## COLOR SYSTEM

Create an 11-stop color palette for each theme:
- 50: Lightest (highlights, icon symbols)
- 100-200: Light tones (gradient light end)
- 300-400: Mid-light (gradient transitions)
- 500: True/primary color
- 600-700: Mid-dark (gradient transitions)
- 800-900: Dark tones (gradient dark end)
- 950: Darkest (shadows, backgrounds)

Background gradient: dark at bottom, lighter at top (matches lighting direction)

## SIDEBAR ICONS (if applicable)

Create matching monochromatic sidebar icons:
- Same clay material technique, scaled to 32x32px
- Derive colors from main theme (--icon-light, --icon-base, --icon-dark)
- Use stroke-based SVG icons (not filled)
- Stroke the icons with the darkest palette color at 60% opacity
- Add text-shadow highlight: drop-shadow(0 1px 0 rgba(255,255,255,0.3))

## BACKGROUND ATMOSPHERE

Add ambient background elements:
- 2-3 large gradient orbs (400-600px diameter)
- filter: blur(80-100px)
- opacity: 0.4-0.5
- Slow drift animation (20-26 seconds)
- Position partially off-screen for organic edges

Optional ambient particles matching the theme:
- Floating particles (rising)
- Sparkles (pulsing)
- Data streams (falling)
- Leaves (drifting)

## TYPOGRAPHY

- Font stack: 'SF Pro Display', 'Inter', -apple-system, BlinkMacSystemFont, sans-serif
- Text colors using rgba white with varying opacity:
  - Primary: rgba(255,255,255,0.95)
  - Secondary: rgba(255,255,255,0.7)
  - Muted: rgba(255,255,255,0.45)
- Slight negative letter-spacing on headings (-0.3 to -0.5px)

## ANIMATIONS & TRANSITIONS

- Panel entrance: fade up + scale (0.8s, cubic-bezier(0.16, 1, 0.3, 1))
- List items: staggered delays (0.1s increment)
- Hover states: subtle lift (translateY or translateX)
- All animations use transform and opacity only (GPU accelerated)

## WHAT TO AVOID

- Hard, sharp edges on icons (everything should feel soft)
- Plastic-looking materials (missing inner shadows)
- Reflections that extend outside the shape bounds
- Flat gradients with only 2 stops
- Jarring, fast animations
- Generic flat icons (all icons should have the 3D clay treatment)
- Using random colors for sidebar icons (keep monochromatic)

## ICON SHAPES TO SUPPORT

Different shapes for different contexts:
- Rounded square (border-radius: 48-52px on ~200px icon)
- Organic blob (animated border-radius morphing)
- Hexagon (clip-path: polygon)
- Shield (clip-path: polygon)
- Pill/capsule (border-radius: 85-90px)
- Circle (border-radius: 50%)

## SPECIFIC CONTENT FOR THIS BUILD

[DESCRIBE YOUR SPECIFIC CONTENT HERE]

Examples:
- "A performance monitoring panel with a lightning bolt icon, showing 5 maintenance tasks with progress indicators"
- "A file browser with a folder icon, showing a path and stop button"
- "A security status panel with a shield icon, showing protection status and a progress ring"

Color theme: [copper/orange | teal/cyan | purple/violet | green | blue]

Icon shape: [rounded-square | blob | hexagon | shield | pill]

Icon symbol: [lightning bolt | folder | shield/checkmark | leaf | cloud]

Layout: [centered single icon | icon + task list | icon + stats grid]
```

---

## Quick Version (Shorter Prompt)

If you need a shorter version:

```
Build a premium macOS-style web UI with a floating 3D icon that looks like polished clay/ceramic.

Technical approach:
1. Multi-layer 3D stack using CSS transform: translateZ() with perspective
2. Clay material = multi-stop gradients + multiple inset box-shadows + ambient glow
3. Contained reflections using CSS mask-image
4. Glass morphism panel with backdrop-filter: blur(50px)
5. Floating animation (6-8s) combining translateY + subtle rotateX/rotateY
6. Mouse parallax with smooth interpolation (lerp factor 0.07)
7. Monochromatic color theme throughout

The icon layers (back to front):
- Ground shadow (ellipse, animates with float)
- Base shape (main gradient + external shadows)
- Depth layer (inset shadows only)
- Glaze (subtle white gradient overlay)
- Reflection (masked crescent highlight)
- Symbol (lightest color, drop-shadow)

Key to "clay not plastic": inner shadows + 6-8 gradient stops + contained reflections

Theme: [your color]
Icon shape: [rounded-square/blob/hexagon/shield/pill]
Content: [your specific UI content]
```

---

## Adaptation Notes

When adapting this prompt:

1. **Change the content section** to describe what you actually want to build
2. **Pick a color theme** from: copper, teal, purple, green, blue (or define your own 11-stop palette)
3. **Choose an icon shape** that matches your content's meaning
4. **Specify the layout** you need (centered, side-by-side, with sidebar, etc.)
5. **Add any specific interactions** (progress bars, task lists, file uploads, etc.)

The core techniques remain the same regardless of content.

---

## Example Adaptations

### For a Music Player:
```
Content: A music player with album art as the 3D floating element
Color theme: Deep purple/violet gradient
Icon shape: Rounded square (like album cover)
Symbol: Musical note or play button
Layout: Centered icon with track info below, playback controls
```

### For a Weather App:
```
Content: Weather display with animated cloud/sun icon
Color theme: Sky blue gradient
Icon shape: Organic blob (cloud-like)
Symbol: Sun, cloud, or weather condition
Layout: Large centered icon with temperature and forecast below
```

### For a Download Manager:
```
Content: Download progress with animated arrow icon
Color theme: Green gradient
Icon shape: Circle or rounded square
Symbol: Downward arrow
Layout: Icon with progress bar and file list
```

---

## Checklist for AI

When building, ensure you have:

- [ ] perspective on container (1000-1200px)
- [ ] transform-style: preserve-3d on wrapper
- [ ] 6 distinct layers with different translateZ values
- [ ] Multi-stop gradient on base (6-8 stops)
- [ ] Inner shadows on depth layer (top highlight, bottom shadow, rim light)
- [ ] Masked reflection (doesn't bleed outside shape)
- [ ] Floating animation with 5 keyframe points
- [ ] Shadow animation synchronized with float
- [ ] Mouse parallax with interpolation
- [ ] Glass panel with backdrop-filter
- [ ] 11-stop color palette
- [ ] Background gradient orbs with blur
- [ ] Staggered entrance animations
- [ ] Smooth hover transitions

---

*This prompt template was created based on successfully building the glass-ui-demo project, which achieved approximately 85-90% of CleanMyMac's visual quality using only CSS/HTML/JS.*
