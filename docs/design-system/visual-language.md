# Design System: Visual Language

## Design Philosophy

**Core Principles:**
1. **Calm by default** - No jarring colors, sudden movements, or aggressive UI
2. **Minimal cognitive load** - Show only what's necessary
3. **Natural rhythms** - Animations mirror breath and organic movement
4. **Adaptive aesthetics** - Visuals soften as calmness increases (future)

---

## Color Palette

### Primary Colors

```
Calm Blue (Primary)
├─ #6B9BD1  Main blue (buttons, accents, breathing circle)
├─ #A8C5E0  Light blue (circle expanded state)
└─ #4A7BA7  Dark blue (pressed states, emphasis)

Neutral Grays
├─ #F5F7FA  Background light
├─ #E8EDF2  Background medium
├─ #E0E7ED  Dividers, unfilled tracks
├─ #5A6C7D  Secondary text
└─ #2C3E50  Primary text

State Colors
├─ #4CAF50  Success / Calm (green)
├─ #FFA726  Neutral / Scattered (orange)
└─ #EF5350  Warning / Very scattered (red)
```

### Usage Guidelines

**Background:**
- Default: Vertical gradient from `#F5F7FA` to `#E8EDF2`
- Alternative: Solid `#F5F7FA` for performance

**Text:**
- Primary: `#2C3E50` (dark gray, not pure black)
- Secondary: `#5A6C7D` (medium gray for labels)
- Hint text: `#5A6C7D` at 70% opacity

**Interactive Elements:**
- Default: `#6B9BD1`
- Hover/Focus: `#4A7BA7`
- Disabled: `#E0E7ED`

**Feedback:**
- Calm improvement: `#4CAF50`
- Neutral: `#FFA726`
- Scattered: `#EF5350`

---

## Typography

### Font Family

**System Default (MVP):**
- iOS: SF Pro
- Android: Roboto
- Web: System UI font stack

**Future (v0.3+):** Custom font for branding (e.g., Inter, DM Sans)

### Type Scale

```
Display Large:  32sp / 38sp line height / Bold
Display Medium: 24sp / 30sp / Semibold

Heading 1:      20sp / 26sp / Semibold
Heading 2:      18sp / 24sp / Medium

Body Large:     16sp / 24sp / Regular
Body Medium:    14sp / 20sp / Regular
Body Small:     12sp / 18sp / Regular

Label:          14sp / 20sp / Medium
Caption:        12sp / 16sp / Regular
```

### Text Hierarchy

**Screen Titles:**
- Display Medium (24sp, Semibold)
- Color: `#2C3E50`
- Usage: "Welcome to MindFool.Games"

**Section Headers:**
- Heading 1 (20sp, Semibold)
- Color: `#2C3E50`
- Usage: "How scattered or calm is your mind?"

**Body Text:**
- Body Large (16sp, Regular)
- Color: `#2C3E50`
- Usage: Descriptions, instructions

**Labels:**
- Label (14sp, Medium)
- Color: `#5A6C7D`
- Usage: Slider endpoints, button labels

**Captions:**
- Caption (12sp, Regular)
- Color: `#5A6C7D` at 70%
- Usage: Timestamps, metadata

---

## Spacing & Layout

### Base Unit: 4dp

**Spacing Scale:**
```
xs:   4dp   (tight padding)
sm:   8dp   (compact spacing)
md:   16dp  (default spacing)
lg:   24dp  (section spacing)
xl:   32dp  (screen margins)
2xl:  48dp  (major section breaks)
```

### Screen Margins

**Horizontal:**
- Phone: 20dp (xl - 12dp for alignment)
- Tablet: 32dp (xl)

**Vertical:**
- Top safe area: Auto (device-specific)
- Bottom safe area: Auto (device-specific)
- Between sections: 24dp (lg)

### Component Padding

**Buttons:**
- Horizontal: 24dp (lg)
- Vertical: 12dp (md - 4dp)

**Cards:**
- All sides: 16dp (md)

**Input fields:**
- Horizontal: 16dp (md)
- Vertical: 12dp (md - 4dp)

---

## Iconography

### Style

**MVP:** System icons (Ionicons from `@expo/vector-icons`)
- Consistent stroke width: 2dp
- Rounded corners
- 24dp default size

**Future:** Custom icon set with organic, rounded style

### Icon Sizes

```
Small:   16dp  (inline with text)
Medium:  24dp  (default UI)
Large:   32dp  (prominent actions)
XLarge:  48dp  (feature icons)
```

### Common Icons

| Icon | Usage | Size |
|------|-------|------|
| ✕ (close) | Exit button | 24dp |
| ▶ (play) | Start session | 32dp |
| ⟲ (refresh) | Play again | 24dp |
| ☰ (menu) | Settings | 24dp |
| ✓ (checkmark) | Confirmation | 20dp |

---

## Shadows & Elevation

### Shadow Tokens

```
None:       No shadow
Subtle:     0 1dp 2dp rgba(0,0,0,0.05)
Soft:       0 2dp 4dp rgba(0,0,0,0.08)
Medium:     0 4dp 8dp rgba(0,0,0,0.10)
Strong:     0 8dp 16dp rgba(0,0,0,0.12)
```

### Usage

**Cards:**
- Default: Soft shadow
- Hover (future web): Medium shadow

**Buttons:**
- Default: Subtle shadow
- Pressed: None (elevation 0)

**Modals:**
- Strong shadow (lifted above content)

**Breathing Circle:**
- Custom glow (blur mask in Skia)
- Not traditional shadow

---

## Border Radius

### Radius Scale

```
xs:   2dp   (subtle rounding)
sm:   4dp   (small elements)
md:   8dp   (default buttons, inputs)
lg:   12dp  (cards)
xl:   16dp  (prominent cards)
full: 9999dp (pills, breathing circle)
```

### Usage

**Buttons:** `md` (8dp)
**Input fields:** `md` (8dp)
**Cards:** `lg` (12dp)
**Breathing circle:** `full` (perfectly round)
**Slider thumb:** `full` (perfectly round)

---

## Animation & Motion

### Animation Principles

1. **Purposeful:** Every animation serves a function (feedback, transition, guidance)
2. **Natural:** Mimic organic movement (ease curves, not linear)
3. **Subtle:** No sudden jerks or aggressive motion
4. **Consistent:** Same timing for same action types

### Timing

**Micro-interactions:**
- Duration: 150-200ms
- Easing: Ease-out
- Usage: Button press, slider drag

**Transitions:**
- Duration: 300-400ms
- Easing: Ease-in-out
- Usage: Screen navigation

**Breathing animations:**
- Duration: 4000ms (inhale), 6000ms (exhale)
- Easing: Custom cubic-bezier
- Usage: Breathing circle

### Easing Functions

```typescript
// Quick interactions
easeOut: cubic-bezier(0.0, 0.0, 0.2, 1.0)

// Screen transitions
easeInOut: cubic-bezier(0.4, 0.0, 0.2, 1.0)

// Breathing (custom, more organic)
breathEase: cubic-bezier(0.45, 0.05, 0.55, 0.95)
```

### Animation Tokens

```typescript
const ANIMATIONS = {
  quick: {
    duration: 150,
    easing: Easing.out(Easing.cubic),
  },
  standard: {
    duration: 300,
    easing: Easing.inOut(Easing.cubic),
  },
  breathing: {
    inhale: {
      duration: 4000,
      easing: Easing.out(Easing.cubic),
    },
    exhale: {
      duration: 6000,
      easing: Easing.in(Easing.cubic),
    },
  },
};
```

---

## Component Patterns

### Buttons

**Primary Button:**
```
Background: #6B9BD1
Text: #FFFFFF
Padding: 12dp vertical, 24dp horizontal
Border radius: 8dp
Shadow: Subtle
Font: 16sp, Medium

States:
- Pressed: Background #4A7BA7, Shadow none
- Disabled: Background #E0E7ED, Text #5A6C7D at 50%
```

**Secondary Button:**
```
Background: Transparent
Text: #6B9BD1
Border: 1dp solid #6B9BD1
Padding: 12dp vertical, 24dp horizontal
Border radius: 8dp
Font: 16sp, Medium
```

### Input Fields

**Text Input:**
```
Background: #FFFFFF
Border: 1dp solid #E0E7ED
Padding: 12dp vertical, 16dp horizontal
Border radius: 8dp
Font: 16sp, Regular
Placeholder color: #5A6C7D at 50%

States:
- Focus: Border #6B9BD1, Shadow subtle
- Error: Border #EF5350
```

### Cards

**Session Card:**
```
Background: #FFFFFF
Padding: 16dp
Border radius: 12dp
Shadow: Soft
Border: None (shadow provides separation)
```

---

## Responsive Breakpoints

**Phone (default):**
- Width: 375-428dp
- Layout: Single column
- Margins: 20dp

**Tablet (future):**
- Width: 768dp+
- Layout: Centered content, max-width 600dp
- Margins: 32dp

**Web (PWA, future):**
- Width: 1024dp+
- Layout: Centered, max-width 480dp (mobile-first)
- Margins: Auto

---

## Accessibility

### Minimum Touch Targets

**All interactive elements:** 48dp × 48dp (WCAG AAA)
- Even if visual size is smaller, tap area must be 48dp

### Color Contrast

**Text on background:**
- Primary text (#2C3E50) on #F5F7FA: 9.8:1 (AAA)
- Secondary text (#5A6C7D) on #F5F7FA: 5.2:1 (AA)

**Interactive elements:**
- Button text (#FFFFFF) on #6B9BD1: 4.6:1 (AA)

### Focus Indicators

**Keyboard navigation (future web):**
- 2dp outline in `#6B9BD1`
- 2dp offset from element

---

## Dark Mode (Post-MVP)

**Future consideration:** Inverted palette
- Background: `#1A1D23` (dark blue-gray)
- Primary text: `#E8EDF2`
- Accent: `#A8C5E0` (lighter blue for contrast)

---

## Implementation Notes

### NativeWind Setup

```typescript
// tailwind.config.js
module.exports = {
  theme: {
    colors: {
      primary: '#6B9BD1',
      'primary-light': '#A8C5E0',
      'primary-dark': '#4A7BA7',
      calm: '#4CAF50',
      neutral: '#FFA726',
      scattered: '#EF5350',
      // ... etc
    },
    spacing: {
      xs: '4px',
      sm: '8px',
      md: '16px',
      lg: '24px',
      xl: '32px',
      '2xl': '48px',
    },
    borderRadius: {
      xs: '2px',
      sm: '4px',
      md: '8px',
      lg: '12px',
      xl: '16px',
      full: '9999px',
    },
  },
};
```

### Design Tokens File

```typescript
// src/constants/tokens.ts
export const COLORS = {
  primary: '#6B9BD1',
  primaryLight: '#A8C5E0',
  primaryDark: '#4A7BA7',
  // ...
};

export const SPACING = {
  xs: 4,
  sm: 8,
  md: 16,
  // ...
};

export const TYPOGRAPHY = {
  displayLarge: { fontSize: 32, lineHeight: 38, fontWeight: '700' },
  // ...
};
```

---

## Future Enhancements

**v0.3:** Custom font integration (Inter or DM Sans)
**v0.4:** Adaptive color intensity (softer colors as user gets calmer)
**v0.5:** Dark mode support
**v1.0:** Accessibility refinements (screen reader optimization, voice control)
