# Technical Stack

## Core Framework

### **Expo SDK 52** (Latest Stable)
- **Why:** Best-in-class developer experience for React Native
- **Benefits:**
  - OTA updates via EAS
  - Simplified build process
  - Strong web support for PWA
  - Rich ecosystem of quality libraries

### **TypeScript**
- **Why:** Type safety prevents runtime errors, improves maintainability
- **Benefits:**
  - Better IDE support
  - Catches errors at compile time
  - Self-documenting code
  - Easier refactoring

---

## Navigation & Routing

### **Expo Router (v3+)**
- File-based routing
- Type-safe navigation
- Deep linking support
- Web-compatible

---

## UI & Animation

### **react-native-skia**
- High-performance canvas rendering
- Perfect for smooth breathing circle animations
- GPU-accelerated
- Cross-platform (iOS, Android, Web)

### **NativeWind (Tailwind for React Native)**
- **Why:** Rapid UI development with consistent styling
- **Benefits:**
  - Familiar Tailwind syntax
  - Responsive design utilities
  - Dark mode support (future)
  - Smaller bundle size than styled-components

### **expo-linear-gradient**
- Subtle gradient backgrounds
- Visual calmness enhancement

---

## State Management

### **Zustand**
- **Why:** Simpler than Redux, perfect for small-to-medium apps
- **Benefits:**
  - Minimal boilerplate
  - TypeScript-first
  - No context provider wrapping
  - Easy to test

**Stores:**
- `useSessionStore`: Pre/post scores, current session state
- `useHistoryStore`: Past session data
- `useSettingsStore`: Future app settings

---

## Data Persistence

### **@react-native-async-storage/async-storage**
- Simple key-value storage
- Perfect for MVP session history
- Zustand middleware for auto-persistence

### **expo-sqlite** (Future)
- For complex queries once we have 100+ sessions
- Not needed for MVP

---

## Audio

### **expo-av**
- Simple audio playback
- Preload sounds for instant feedback
- **Sounds needed:**
  - Inhale peak tone (soft, high)
  - Exhale complete tone (soft, low)
  - Session start chime
  - Session end chime

---

## Haptics

### **expo-haptics** (Post-MVP)
- Subtle vibration feedback
- Reinforce breath rhythm
- Not in MVP to reduce complexity

---

## Analytics & Monitoring

### **Expo Application Services (EAS)**
- Crash reporting
- Build management
- OTA updates

### **Custom Analytics** (Post-MVP)
- Privacy-first approach
- Only track: sessions completed, pre/post deltas, mode usage
- NO personal data, NO tracking outside app

---

## Development Tools

### **ESLint + Prettier**
- Code consistency
- Auto-formatting

### **TypeScript Strict Mode**
- Maximum type safety

### **React Native Debugger / Flipper**
- Debugging tools

---

## Build & Deployment

### **EAS Build**
- Cloud-based builds for Android
- Automatic signing

### **EAS Submit**
- Simplified Play Store submission

### **GitHub Actions** (Future)
- Automated testing
- Automated builds on commit

---

## Testing (Post-MVP)

### **Jest**
- Unit tests for game logic

### **React Native Testing Library**
- Component tests

### **Detox** (Future)
- E2E testing on real devices

---

## Performance Considerations

**Target:**
- 60fps animations (breathing circle)
- < 50MB app size
- < 2s cold start time
- < 100ms tap-to-feedback latency

**Optimization strategies:**
- Lazy load non-critical screens
- Preload audio files
- Optimize Skia canvas rendering
- Memoize expensive calculations

---

## Web (PWA) Considerations

**Future Release:**
- `@expo/webpack-config` for web builds
- Same codebase, dual deployment
- Touch/mouse event handling
- Responsive layout adjustments

---

## Dependencies Summary

```json
{
  "dependencies": {
    "expo": "~52.0.0",
    "react": "18.3.1",
    "react-native": "0.76.1",
    "expo-router": "~4.0.0",
    "@shopify/react-native-skia": "^1.4.0",
    "nativewind": "^4.0.0",
    "zustand": "^5.0.0",
    "@react-native-async-storage/async-storage": "^2.0.0",
    "expo-av": "~15.0.0",
    "expo-linear-gradient": "~14.0.0",
    "expo-haptics": "~14.0.0"
  },
  "devDependencies": {
    "typescript": "~5.6.2",
    "@types/react": "~18.3.0",
    "eslint": "^8.57.0",
    "prettier": "^3.0.0"
  }
}
```

---

## Architecture Decision Records (ADRs)

**ADR-001: Why Zustand over Redux?**
- Simpler API, less boilerplate
- App state is straightforward (no complex side effects)
- TypeScript integration is cleaner
- Smaller bundle size

**ADR-002: Why Skia over Reanimated?**
- Better for canvas-based animations (breathing circle)
- Web compatibility via CanvasKit
- More control over rendering

**ADR-003: Why NativeWind over StyleSheet?**
- Faster iteration during MVP phase
- Consistent with modern web practices
- Easier for contributors familiar with Tailwind
- Better responsive design utilities
