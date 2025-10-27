# Feature Specification: Balloon Breathing

## Overview

**Purpose:** A visual breathing guide that helps users synchronize their breath with a gentle animation, promoting calm and presence.

**Inspired by:** Belly breathing / diaphragmatic breathing practices

**Duration:** 2-3 minutes (configurable)

---

## User Experience Flow

```
1. User navigates from HomeScreen (after setting pre-score)
   ↓
2. Brief transition animation (0.5s)
   ↓
3. Game screen appears:
   - Centered breathing circle (initial state: medium size)
   - Subtle background gradient
   - Minimal UI (timer, exit button)
   ↓
4. After 1 second, session-start chime plays
   ↓
5. Circle begins first INHALE cycle:
   - Expands smoothly over 4 seconds
   - Subtle glow intensifies
   - At peak: soft high-pitched tone
   ↓
6. Circle transitions to EXHALE cycle:
   - Contracts smoothly over 6 seconds
   - Glow softens
   - At complete contraction: soft low-pitched tone
   ↓
7. Cycle repeats for 2-3 minutes (12-18 breath cycles)
   ↓
8. Session-end chime plays
   ↓
9. Auto-navigate to PostGameScreen
```

---

## Visual Specifications

### Breathing Circle

**Technology:** `@shopify/react-native-skia`

#### States

| State | Diameter | Color | Glow |
|-------|----------|-------|------|
| Minimum (exhale complete) | 100dp | `#6B9BD1` (soft blue) | 0dp blur |
| Maximum (inhale peak) | 220dp | `#A8C5E0` (lighter blue) | 20dp blur |

#### Animation Curve

- **Inhale:** Ease-out (accelerates at start, slows at peak)
- **Exhale:** Ease-in (slow at start, gentle acceleration)
- **Timing Function:** `cubic-bezier(0.4, 0.0, 0.2, 1.0)`

#### Transition Timing

```
Inhale:  4.0 seconds
Exhale:  6.0 seconds
Total cycle: 10 seconds
Total cycles: 12-18 (2-3 minutes)
```

### Background

- **Gradient:** Vertical linear gradient
  - Top: `#F5F7FA` (very light blue-gray)
  - Bottom: `#E8EDF2` (slightly darker blue-gray)
- **Effect:** Calm, neutral, non-distracting

### UI Elements

**Timer (top-right):**
- Countdown format: `2:45` (MM:SS)
- Font: System default, 18sp
- Color: `#6B9BD1` (matches circle)
- Opacity: 0.7 (subtle)

**Exit Button (top-left):**
- Simple "×" icon
- Size: 32dp
- Color: `#6B9BD1`
- Tap area: 48dp (accessible)
- **Action:** Confirm modal → Exit session (data lost)

---

## Audio Specifications

### Sound Files

**Format:** MP3, 48kHz, mono, normalized to -12dB

| Sound | Trigger | Duration | Description |
|-------|---------|----------|-------------|
| `session-start.mp3` | Session begins | 1.5s | Soft bell chime (D note) |
| `inhale-peak.mp3` | Circle reaches max | 0.8s | High tone (A note, gentle) |
| `exhale-complete.mp3` | Circle reaches min | 0.8s | Low tone (E note, warm) |
| `session-end.mp3` | Session completes | 2.0s | Three-tone chime (E-G-C) |

### Audio Behavior

- **Preload:** All sounds loaded on app launch
- **Overlap:** Prevent overlapping sounds (cancel previous if new triggered)
- **Fallback:** If audio fails, continue with visual-only mode
- **Volume:** Default 70%, adjustable in settings (post-MVP)

---

## Interaction Specifications

### Tap-to-Sync (Optional Enhancement - Post-MVP)

**Goal:** User taps screen to match breath rhythm (validates synchronization)

**Behavior:**
- Tap during inhale → Gentle haptic feedback
- Tap during exhale → Different haptic pattern
- If rhythm matches (±0.5s tolerance) → Visual confirmation (subtle pulse)
- **Scoring:** Calculate smoothness based on tap variance

**MVP Decision:** Skip tap interaction for simplicity. Pure observation mode only.

---

## Data Tracking

### Session Metrics

```typescript
interface BreathingSession {
  sessionId: string;
  mode: 'balloon-breathing';
  preScore: number;
  postScore: number;
  delta: number;
  duration: number; // milliseconds
  cyclesCompleted: number;
  notes?: string;
  timestamp: Date;

  // Future metrics (not MVP):
  // tapVariance?: number;
  // averageBreathLength?: number;
}
```

### Calculation Logic

**Delta:**
```typescript
delta = postScore - preScore;
// Positive = calmer, Negative = more scattered
```

**Cycles Completed:**
```typescript
cyclesCompleted = Math.floor(duration / 10000); // 10s per cycle
```

---

## Technical Implementation

### Component Structure

```typescript
// BreathingCircle.tsx
import { Canvas, Circle, BlurMask } from '@shopify/react-native-skia';
import { useSharedValue, withTiming, Easing } from 'react-native-reanimated';

const BreathingCircle = () => {
  const diameter = useSharedValue(100); // Start at minimum
  const glowRadius = useSharedValue(0);

  // Inhale animation
  const startInhale = () => {
    diameter.value = withTiming(220, {
      duration: 4000,
      easing: Easing.out(Easing.cubic),
    });
    glowRadius.value = withTiming(20, { duration: 4000 });
  };

  // Exhale animation
  const startExhale = () => {
    diameter.value = withTiming(100, {
      duration: 6000,
      easing: Easing.in(Easing.cubic),
    });
    glowRadius.value = withTiming(0, { duration: 6000 });
  };

  // Cycle management handled by breathTimer service
};
```

### Breath Timer Service

```typescript
// breathTimer.ts
export class BreathTimer {
  private intervalId: NodeJS.Timeout | null = null;
  private currentPhase: 'inhale' | 'exhale' = 'inhale';

  start(onInhale: () => void, onExhale: () => void) {
    // Start with inhale
    onInhale();
    this.currentPhase = 'inhale';

    // Switch phases every cycle
    this.intervalId = setInterval(() => {
      if (this.currentPhase === 'inhale') {
        onExhale();
        this.currentPhase = 'exhale';
      } else {
        onInhale();
        this.currentPhase = 'inhale';
      }
    }, this.currentPhase === 'inhale' ? 4000 : 6000);
  }

  stop() {
    if (this.intervalId) clearInterval(this.intervalId);
  }
}
```

---

## Edge Cases & Error Handling

| Scenario | Behavior |
|----------|----------|
| User exits mid-session | Show confirmation modal: "Exit? Progress won't be saved." |
| Audio fails to load | Continue with visual-only mode (no errors shown) |
| App goes to background | Pause timer and animation, resume on return |
| Session duration exceeded | Auto-end session, navigate to PostGameScreen |
| Timer drift > 500ms | Reset cycle (rare, but prevents desynced audio/visual) |

---

## Accessibility Considerations

**Screen Reader:**
- Announce: "Breathing circle. Follow the animation with your breath."
- Announce phase changes: "Inhale" / "Exhale"

**Reduced Motion:**
- Respect system `prefers-reduced-motion` setting
- Alternative: Static circle with text "Inhale" / "Exhale"

**Color Contrast:**
- Ensure circle visible for colorblind users (blue chosen for universality)

---

## Success Metrics

**Technical:**
- 60fps animation (measured via Flipper)
- < 50ms audio latency
- 0 crashes during 100 test sessions

**User Experience:**
- Average delta: +2 or higher (users feel calmer)
- Session completion rate: > 80%
- Qualitative feedback: "The rhythm felt natural"

---

## Future Enhancements (Post-MVP)

- **Customizable breath ratios:** 4:6, 4:7, 5:5
- **Background soundscapes:** Ocean, forest, rain
- **Haptic feedback:** Subtle vibration at inhale/exhale peaks
- **Guided voice:** Optional voice cues "Breathe in... Breathe out..."
- **Visual themes:** Different color palettes (warm, cool, neutral)
