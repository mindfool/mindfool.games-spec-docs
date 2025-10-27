# System Architecture Overview

## High-Level Architecture

```
┌─────────────────────────────────────────────────────┐
│                   MindFool.Games                    │
│                 (React Native App)                  │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────┐ │
│  │  Screens    │  │  Components  │  │  Services │ │
│  │             │  │              │  │           │ │
│  │ - Home      │  │ - Breathing  │  │ - Audio   │ │
│  │ - Game      │  │   Circle     │  │ - Storage │ │
│  │ - PostGame  │  │ - Slider     │  │ - Timer   │ │
│  └──────┬──────┘  └──────┬───────┘  └─────┬─────┘ │
│         │                │                 │       │
│         └────────────────┼─────────────────┘       │
│                          │                         │
│                  ┌───────▼────────┐                │
│                  │  State Stores  │                │
│                  │   (Zustand)    │                │
│                  │                │                │
│                  │ - Session      │                │
│                  │ - History      │                │
│                  └───────┬────────┘                │
│                          │                         │
│                  ┌───────▼────────┐                │
│                  │  Persistence   │                │
│                  │ (AsyncStorage) │                │
│                  └────────────────┘                │
└─────────────────────────────────────────────────────┘
```

---

## Directory Structure

```
mindfool.games/
├── app/                          # Expo Router screens
│   ├── (tabs)/                  # Tab-based navigation (future)
│   ├── index.tsx                # Home screen
│   ├── game.tsx                 # Game session screen
│   └── post-game.tsx            # Post-game reflection
│
├── src/
│   ├── components/              # Reusable UI components
│   │   ├── BreathingCircle.tsx # Skia animated circle
│   │   ├── CalmSlider.tsx      # Pre/post check-in slider
│   │   ├── DeltaDisplay.tsx    # Show improvement
│   │   └── ReflectionInput.tsx # Optional notes
│   │
│   ├── stores/                  # Zustand state management
│   │   ├── sessionStore.ts     # Current session state
│   │   └── historyStore.ts     # Past sessions
│   │
│   ├── services/                # Business logic
│   │   ├── audioService.ts     # Sound playback
│   │   ├── storageService.ts   # AsyncStorage wrapper
│   │   └── breathTimer.ts      # Breath cycle timing
│   │
│   ├── types/                   # TypeScript definitions
│   │   └── index.ts            # Shared types
│   │
│   ├── constants/               # App constants
│   │   ├── colors.ts           # Color palette
│   │   ├── timing.ts           # Animation/breath timing
│   │   └── sounds.ts           # Audio file paths
│   │
│   └── utils/                   # Helper functions
│       ├── deltaCalculator.ts  # Pre/post comparison
│       └── sessionLogger.ts    # Session data formatter
│
├── assets/
│   ├── sounds/                  # Audio files
│   │   ├── inhale-peak.mp3
│   │   ├── exhale-complete.mp3
│   │   ├── session-start.mp3
│   │   └── session-end.mp3
│   │
│   └── fonts/                   # Custom fonts (if any)
│
├── docs/                        # Additional documentation
├── .env                         # Environment variables
├── app.json                     # Expo config
├── package.json
└── tsconfig.json
```

---

## Data Flow

### Session Flow

```
1. User opens app
   ↓
2. HomeScreen renders
   ├─→ Reads lastSession from historyStore
   └─→ Displays CalmSlider component
   ↓
3. User sets pre-session calmness (0-10)
   ↓
4. sessionStore.startSession(preScore)
   ├─→ Generates session ID (timestamp)
   └─→ Stores preScore in sessionStore
   ↓
5. Navigate to GameScreen
   ↓
6. BreathingCircle component renders
   ├─→ breathTimer service controls cycle
   ├─→ Skia animates circle
   └─→ audioService plays cues
   ↓
7. User completes 2-3 minute session
   ↓
8. Navigate to PostGameScreen
   ↓
9. User sets post-session calmness (0-10)
   ↓
10. sessionStore.endSession(postScore, notes)
    ├─→ Calculates delta
    ├─→ Saves to historyStore
    └─→ historyStore persists to AsyncStorage
    ↓
11. Display DeltaDisplay component
    ↓
12. User taps "Play Again" or returns home
```

---

## State Management

### Session Store

```typescript
interface SessionState {
  sessionId: string | null;
  preScore: number | null;
  postScore: number | null;
  startTime: Date | null;
  endTime: Date | null;
  mode: 'balloon-breathing'; // Hardcoded for MVP

  // Actions
  startSession: (preScore: number) => void;
  endSession: (postScore: number, notes?: string) => void;
  resetSession: () => void;
}
```

### History Store

```typescript
interface HistoryState {
  sessions: Session[];

  // Actions
  addSession: (session: Session) => void;
  getSessions: () => Session[];
  getLastSession: () => Session | null;
}

interface Session {
  id: string;
  mode: 'balloon-breathing';
  preScore: number;
  postScore: number;
  delta: number;
  duration: number; // milliseconds
  notes?: string;
  timestamp: Date;
}
```

---

## Component Hierarchy

```
App (Expo Router)
│
├── app/index.tsx (HomeScreen)
│   ├── CalmSlider (pre-session)
│   ├── LastSessionCard
│   └── StartButton
│
├── app/game.tsx (GameScreen)
│   ├── BreathingCircle (Skia canvas)
│   ├── SessionTimer
│   └── ExitButton
│
└── app/post-game.tsx (PostGameScreen)
    ├── CalmSlider (post-session)
    ├── DeltaDisplay
    ├── ReflectionInput
    └── PlayAgainButton
```

---

## Service Interfaces

### Audio Service

```typescript
interface AudioService {
  preload(): Promise<void>;
  playInhalePeak(): void;
  playExhaleComplete(): void;
  playSessionStart(): void;
  playSessionEnd(): void;
  cleanup(): void;
}
```

### Breath Timer Service

```typescript
interface BreathTimer {
  start(onInhale: () => void, onExhale: () => void): void;
  stop(): void;
  getCurrentPhase(): 'inhale' | 'exhale';
}
```

### Storage Service

```typescript
interface StorageService {
  saveSession(session: Session): Promise<void>;
  getSessions(): Promise<Session[]>;
  clearAll(): Promise<void>; // For testing
}
```

---

## Performance Optimization

### Rendering Strategy

- **Skia Canvas:** Breathing circle runs on UI thread (60fps guaranteed)
- **Zustand:** Minimal re-renders via selector pattern
- **Memoization:** Heavy components memoized with `React.memo`

### Audio Preloading

```typescript
// Preload all sounds on app launch
useEffect(() => {
  audioService.preload();
}, []);
```

### Lazy Loading (Post-MVP)

```typescript
// Future: Lazy load game screens
const GameScreen = lazy(() => import('./app/game'));
```

---

## Error Handling

### Strategy

1. **Audio fails to load:** Fallback to silent mode (visual-only)
2. **Storage fails:** Keep session in memory, retry save on next launch
3. **Timer drift:** Reset breath cycle if > 500ms drift detected

### Error Boundaries

```typescript
// Top-level error boundary for crash recovery
<ErrorBoundary fallback={<ErrorScreen />}>
  <App />
</ErrorBoundary>
```

---

## Future Architecture Considerations

**v0.2+:** Multiple game modes → Router structure for mode selection
**v0.4+:** Adaptive engine → New `adaptiveStore` for mode suggestions
**v1.0+:** Analytics → Separate analytics service with privacy controls
