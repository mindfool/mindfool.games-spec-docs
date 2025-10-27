# Feature Specification: Pre/Post Check-In System

## Overview

**Purpose:** Self-awareness tool that helps users track their mental state before and after each session, creating a measurable feedback loop.

**Core Principle:** Users are the best judges of their own state. The app doesn't tell them how to feel—it mirrors back their self-reported experience.

---

## User Flow

```
PRE-SESSION:
1. User opens app
   ↓
2. Home screen displays: "How scattered or calm is your mind?"
   ↓
3. User moves slider (0-10)
   - Left (0-2): "Calm, focused, present"
   - Middle (3-6): "Somewhat scattered"
   - Right (7-10): "Very scattered, restless"
   ↓
4. Slider value stored → User taps "Start"
   ↓
5. Game begins

POST-SESSION:
6. Game completes
   ↓
7. Post-game screen displays: "How stable and calm do you feel now?"
   ↓
8. User moves slider (0-10) [same scale]
   ↓
9. Delta calculated and displayed
   ↓
10. Optional reflection: "What did you notice?"
   ↓
11. Session saved to history
```

---

## Visual Design

### Slider Component

**Technology:** Custom React Native component (not native Slider for design control)

#### Layout

```
┌─────────────────────────────────────────────┐
│  How scattered or calm is your mind?        │
│                                             │
│  ●━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━○  │
│  ↑                                         ↑ │
│  Very Calm                        Very Scattered
│                                             │
│  Current: 4 - "Somewhat scattered"         │
└─────────────────────────────────────────────┘
```

#### Specifications

**Track:**
- Length: 300dp
- Height: 4dp
- Filled color: `#6B9BD1` (blue, matches theme)
- Unfilled color: `#E0E7ED` (light gray)

**Thumb:**
- Size: 28dp diameter
- Color: `#6B9BD1`
- Shadow: 2dp elevation
- Accessible tap area: 48dp

**Labels:**
- Position: Below track, left/right ends
- Font: 14sp, medium weight
- Color: `#5A6C7D` (subtle gray)

**Value Display:**
- Position: Below slider, centered
- Format: `"4 - Somewhat scattered"`
- Font: 16sp, semibold
- Color: Dynamic based on value:
  - 0-2: `#4CAF50` (green - calm)
  - 3-6: `#FFA726` (orange - neutral)
  - 7-10: `#EF5350` (red - scattered)

---

## Scale Definition

### Value Mapping

| Range | Label | Description | Color |
|-------|-------|-------------|-------|
| 0-1 | "Very calm" | Deep focus, present, quiet mind | Green |
| 2 | "Calm" | Settled, at ease | Green |
| 3-4 | "Somewhat scattered" | Mild distraction, wandering thoughts | Orange |
| 5-6 | "Scattered" | Difficulty focusing, restless | Orange |
| 7-8 | "Very scattered" | Mind racing, fragmented attention | Red |
| 9-10 | "Extremely scattered" | Overwhelmed, chaotic thoughts | Red |

### Label Array (Implementation)

```typescript
const SCATTER_LABELS = [
  "Very calm",        // 0
  "Very calm",        // 1
  "Calm",             // 2
  "Somewhat scattered", // 3
  "Somewhat scattered", // 4
  "Scattered",        // 5
  "Scattered",        // 6
  "Very scattered",   // 7
  "Very scattered",   // 8
  "Extremely scattered", // 9
  "Extremely scattered", // 10
];
```

---

## Delta Display

### Calculation

```typescript
delta = postScore - preScore;

// Examples:
// Pre: 7 (scattered), Post: 4 (somewhat scattered) → Delta: -3 (improved)
// Pre: 3 (somewhat scattered), Post: 6 (scattered) → Delta: +3 (worsened)
```

**Note:** Negative delta = improvement (became calmer)

### Visual Presentation

**Post-Game Screen Layout:**

```
┌─────────────────────────────────────────────┐
│           Session Complete!                 │
│                                             │
│  ╭───────────────────────────────────────╮ │
│  │  Before:  7  "Very scattered"         │ │
│  │  After:   4  "Somewhat scattered"     │ │
│  │                                       │ │
│  │     ✨ You feel 3 points calmer!      │ │
│  ╰───────────────────────────────────────╯ │
│                                             │
│  [Continue button]                          │
└─────────────────────────────────────────────┘
```

#### Delta Messages

| Delta | Message | Icon | Color |
|-------|---------|------|-------|
| ≤ -4 | "Wow! You feel {n} points calmer!" | ✨ | Green |
| -2 to -3 | "You feel {n} points calmer!" | 🌿 | Green |
| -1 | "You feel a bit calmer" | 🍃 | Light Green |
| 0 | "Your calm stayed the same" | • | Gray |
| +1 | "You feel a bit more scattered" | - | Orange |
| ≥ +2 | "You feel {n} points more scattered" | ⚠️ | Orange |

**Note:** Avoid negative framing. Even if delta is positive (worse), keep tone neutral and constructive.

---

## Optional Reflection

**Prompt:** "What did you notice?" (optional text input)

**Purpose:**
- Encourages meta-awareness
- Captures qualitative insights
- No required response (can skip)

**Implementation:**

```
┌─────────────────────────────────────────────┐
│  What did you notice?                       │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │ E.g., "My breathing slowed down," or│   │
│  │ "I noticed tension in my shoulders" │   │
│  │                                     │   │
│  └─────────────────────────────────────┘   │
│                                             │
│  [Skip]  [Save & Continue]                  │
└─────────────────────────────────────────────┘
```

**Character Limit:** 200 characters
**Multiline:** Yes (3 lines visible)
**Storage:** Saved with session data

---

## Data Structure

### Session Object

```typescript
interface CheckInData {
  preScore: number;       // 0-10
  postScore: number;      // 0-10
  delta: number;          // postScore - preScore
  preLabel: string;       // "Very scattered"
  postLabel: string;      // "Somewhat scattered"
  notes?: string;         // Optional reflection
}
```

### Storage Format

```typescript
const session: Session = {
  id: '1698765432000', // timestamp
  mode: 'balloon-breathing',
  preScore: 7,
  postScore: 4,
  delta: -3, // Improvement
  duration: 180000, // 3 minutes in ms
  notes: 'My breathing slowed down',
  timestamp: new Date('2024-01-15T14:30:00'),
};
```

---

## Technical Implementation

### Slider Component

```typescript
// CalmSlider.tsx
import { View, Text } from 'react-native';
import { Gesture, GestureDetector } from 'react-native-gesture-handler';
import Animated, { useSharedValue, useAnimatedStyle } from 'react-native-reanimated';

interface CalmSliderProps {
  value: number;
  onChange: (value: number) => void;
  question: string;
}

export const CalmSlider = ({ value, onChange, question }: CalmSliderProps) => {
  const position = useSharedValue(value * 30); // 0-300dp track

  const panGesture = Gesture.Pan()
    .onChange((event) => {
      const newPos = Math.max(0, Math.min(300, position.value + event.translationX));
      position.value = newPos;
      const newValue = Math.round(newPos / 30); // Convert to 0-10
      onChange(newValue);
    });

  return (
    <View>
      <Text>{question}</Text>
      <GestureDetector gesture={panGesture}>
        {/* Slider UI */}
      </GestureDetector>
      <Text>{value} - {SCATTER_LABELS[value]}</Text>
    </View>
  );
};
```

### Delta Display Component

```typescript
// DeltaDisplay.tsx
interface DeltaDisplayProps {
  preScore: number;
  postScore: number;
}

export const DeltaDisplay = ({ preScore, postScore }: DeltaDisplayProps) => {
  const delta = postScore - preScore;
  const message = getDeltaMessage(delta);
  const color = getDeltaColor(delta);

  return (
    <View style={{ backgroundColor: color }}>
      <Text>Before: {preScore} "{SCATTER_LABELS[preScore]}"</Text>
      <Text>After: {postScore} "{SCATTER_LABELS[postScore]}"</Text>
      <Text>{message}</Text>
    </View>
  );
};
```

---

## Edge Cases

| Scenario | Behavior |
|----------|----------|
| User doesn't move slider | Default to middle (5) with prompt: "Please rate your current state" |
| Pre and post scores identical | Show: "Your calm stayed the same" (neutral) |
| User exits before post-check-in | Session not saved (data lost) |
| Reflection note too long | Truncate at 200 chars with "..." |

---

## Accessibility

**Screen Reader:**
- Slider: "Calmness scale, 0 to 10. Current value: 4, Somewhat scattered."
- Delta: "Before session: 7, Very scattered. After session: 4, Somewhat scattered. You feel 3 points calmer."

**Increased Touch Targets:**
- Slider thumb tap area: 48dp (WCAG AAA compliant)

**Color Contrast:**
- Text on backgrounds meets WCAG AA standards (4.5:1 minimum)

---

## Privacy Considerations

**Data Storage:**
- All check-in data stored locally only (AsyncStorage)
- NO server uploads in MVP
- NO analytics tracking of individual scores

**Future Analytics (opt-in only):**
- Aggregate, anonymized trends (e.g., "80% of users improve by 2+ points")
- User must explicitly consent

---

## Success Metrics

**Completion Rate:**
- Target: > 90% of users complete post-check-in
- If lower, consider: auto-advance after 30s of inactivity

**Delta Distribution:**
- Expected: 70% negative delta (improvement)
- If lower, investigate game effectiveness

**Reflection Usage:**
- Expected: 30-40% of users add notes
- High-quality qualitative data for iteration

---

## Future Enhancements

**v0.2+:**
- Weekly trend graph (7-day rolling average of deltas)
- "Best session" highlight (biggest improvement)
- Patterns detection: "You tend to feel calmer in the evenings"

**v0.3+:**
- Custom scale labels (user can define their own)
- Multiple dimensions: "Calm/Scattered" + "Energized/Tired"

**v0.4+:**
- Adaptive suggestions based on check-in patterns
- "You've been scattered 3 days in a row. Try Gong Listening today."
