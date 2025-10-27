# Implementation Roadmap

## Overview

This roadmap follows the **agent-os methodology**: specifications complete → AI-assisted implementation → iterative refinement.

**Target:** MVP ready for alpha testing in 5 weeks

---

## Week 1: Foundation & Core UI

### Goals
- ✅ Project scaffolding
- ✅ Core navigation structure
- ✅ Design system implementation
- ✅ Home screen with check-in system

### Tasks

**Day 1-2: Project Setup**
- [ ] Initialize Expo app with TypeScript
- [ ] Set up Expo Router (file-based routing)
- [ ] Install core dependencies (Skia, NativeWind, Zustand)
- [ ] Configure ESLint + Prettier
- [ ] Set up basic folder structure (`/src`, `/app`, `/assets`)

**Day 3-4: Design System**
- [ ] Create design tokens file (`src/constants/tokens.ts`)
- [ ] Configure NativeWind with custom theme
- [ ] Build base components:
  - `Button.tsx` (primary/secondary variants)
  - `Card.tsx`
  - `Typography.tsx` (preset text styles)
- [ ] Test design tokens on sample screen

**Day 5-7: Home Screen + Check-In**
- [ ] Build `app/index.tsx` (Home screen)
- [ ] Create `CalmSlider.tsx` component
  - Slider track rendering
  - Thumb dragging gesture
  - Value label display
- [ ] Implement session store (`src/stores/sessionStore.ts`)
  - `startSession(preScore)` action
  - Store pre-score on slider change
- [ ] Wire up "Start" button navigation
- [ ] Test: Pre-score persists during navigation

**Milestone:** User can rate calmness and navigate to game screen

---

## Week 2: Balloon Breathing Game

### Goals
- ✅ Breathing circle animation (Skia)
- ✅ Audio integration
- ✅ Game session timer
- ✅ Navigation to post-game screen

### Tasks

**Day 8-10: Breathing Circle (Skia)**
- [ ] Create `app/game.tsx` (Game screen)
- [ ] Build `BreathingCircle.tsx` component
  - Skia Canvas setup
  - Circle with blur mask (glow effect)
  - Animated diameter (100dp → 220dp)
- [ ] Implement `breathTimer.ts` service
  - 10s cycle management (4s inhale, 6s exhale)
  - Callbacks for phase changes
- [ ] Connect timer to circle animation
- [ ] Test: 60fps animation verified via Flipper

**Day 11-12: Audio System**
- [ ] Add audio files to `/assets/sounds/`
  - `session-start.mp3`
  - `inhale-peak.mp3`
  - `exhale-complete.mp3`
  - `session-end.mp3`
- [ ] Create `audioService.ts`
  - Preload all sounds on app launch
  - Play functions for each sound
  - Cleanup on unmount
- [ ] Wire audio triggers to breath timer
  - Inhale peak → high tone
  - Exhale complete → low tone
- [ ] Test: Audio plays in sync with animation

**Day 13-14: Session Timer & Flow**
- [ ] Add countdown timer UI (top-right corner)
- [ ] Implement 2-3 minute session duration
- [ ] Auto-navigate to post-game screen on completion
- [ ] Add exit button (top-left)
  - Confirmation modal: "Exit? Progress won't be saved."
- [ ] Test: Full session flow (home → game → post)

**Milestone:** Complete breathing session with visuals and audio

---

## Week 3: Post-Game & Data Persistence

### Goals
- ✅ Post-session check-in
- ✅ Delta calculation and display
- ✅ Session history storage
- ✅ Reflection input

### Tasks

**Day 15-16: Post-Game Screen**
- [ ] Build `app/post-game.tsx`
- [ ] Reuse `CalmSlider.tsx` for post-score
- [ ] Create `DeltaDisplay.tsx` component
  - Show pre/post scores with labels
  - Calculate delta
  - Display improvement message
- [ ] Wire post-score to session store
  - `endSession(postScore)` action
- [ ] Test: Delta calculation accuracy

**Day 17-18: History Store & Persistence**
- [ ] Create `historyStore.ts`
  - `sessions: Session[]` array
  - `addSession()` action
  - Zustand persist middleware (AsyncStorage)
- [ ] Create `storageService.ts`
  - AsyncStorage wrapper
  - Save/load session history
- [ ] Wire session completion to history
  - On post-score submit → save to history
- [ ] Test: Data persists across app restarts

**Day 19-20: Reflection & Polish**
- [ ] Create `ReflectionInput.tsx`
  - Optional text input (200 char limit)
  - "Skip" and "Save & Continue" buttons
- [ ] Add reflection to session data model
- [ ] Update home screen:
  - Show last session stats (if exists)
  - Display last delta ("Last time: +3 calmer!")
- [ ] Test: Full data flow (home → game → post → home)

**Day 21: Week 3 Integration Testing**
- [ ] End-to-end test: 5 consecutive sessions
- [ ] Verify all data persists correctly
- [ ] Check performance (no memory leaks)
- [ ] Fix any bugs discovered

**Milestone:** Complete MVP loop with data persistence

---

## Week 4: Polish & Internal Testing

### Goals
- ✅ UI/UX refinements
- ✅ Bug fixes
- ✅ Performance optimization
- ✅ Internal testing (team + friends)

### Tasks

**Day 22-23: Visual Polish**
- [ ] Refine breathing circle aesthetics
  - Adjust glow intensity
  - Smooth animation curves
- [ ] Improve slider feel
  - Add haptic feedback (optional)
  - Refine drag sensitivity
- [ ] Polish typography and spacing
- [ ] Add loading states (if needed)

**Day 24-25: Audio Refinement**
- [ ] Normalize audio levels (all -12dB)
- [ ] Test audio on multiple devices
- [ ] Add fallback for audio load failures
- [ ] Test silent mode behavior (device muted)

**Day 26-27: Performance Optimization**
- [ ] Profile app with React DevTools
- [ ] Optimize Skia rendering (if needed)
- [ ] Reduce bundle size
  - Check unused dependencies
  - Optimize asset sizes
- [ ] Test cold start time (< 2s target)

**Day 28: Internal Testing**
- [ ] Deploy to 5 internal testers (TestFlight/EAS)
- [ ] Collect feedback:
  - Does breathing rhythm feel natural?
  - Is delta meaningful?
  - Any crashes or bugs?
- [ ] Create bug tracking list

**Milestone:** Polished MVP ready for alpha testers

---

## Week 5: Alpha Testing & Iteration

### Goals
- ✅ External alpha testing (10 users)
- ✅ Gather feedback
- ✅ Critical bug fixes
- ✅ Prepare for beta

### Tasks

**Day 29-30: Alpha Deployment**
- [ ] Build production APK (EAS Build)
- [ ] Deploy to 10 alpha testers (Bali/VN group)
- [ ] Send onboarding message:
  - "Complete 3 sessions over 3 days"
  - "Share your honest feedback"
- [ ] Set up feedback form (Google Form or similar)

**Day 31-32: Monitor & Support**
- [ ] Monitor crash reports (Sentry or EAS)
- [ ] Respond to tester questions
- [ ] Collect qualitative feedback:
  - What felt good?
  - What felt off?
  - Would you use this daily?

**Day 33-34: Iteration**
- [ ] Fix critical bugs
- [ ] Implement high-priority feedback
  - E.g., "Breathing too fast" → adjust timing
  - E.g., "Slider too sensitive" → reduce sensitivity
- [ ] Re-test internally

**Day 35: Alpha Wrap-Up**
- [ ] Analyze alpha data:
  - Average delta (target: +2 or higher)
  - Completion rate (target: 80%+)
  - Retention (did they come back for 3 sessions?)
- [ ] Document learnings
- [ ] Create post-MVP feature backlog

**Milestone:** Validated MVP with real user data

---

## Success Criteria (End of Week 5)

### Technical
- ✅ App runs on Android 10+ without crashes
- ✅ 60fps breathing animation on mid-range devices
- ✅ Data persists correctly across sessions
- ✅ Audio plays reliably (or gracefully degrades)

### User Experience
- ✅ 10 alpha testers complete 3 sessions each (30 sessions total)
- ✅ Average post-session delta ≥ +2 (users feel calmer)
- ✅ 80%+ session completion rate
- ✅ 60%+ of users complete all 3 sessions (retention)

### Qualitative
- ✅ "The breathing rhythm felt natural"
- ✅ "I felt calmer after using it"
- ✅ "I would use this daily"

---

## Post-MVP Roadmap (v0.2+)

### v0.2 (Weeks 6-8): Additional Mini-Games
- Walking Meditation implementation
- Number Bubbles implementation
- Mode selection screen

### v0.3 (Weeks 9-11): Advanced Features
- Gong Listening mode
- Counting Ladder mode
- Weekly trend visualization

### v0.4 (Weeks 12-15): Adaptive System
- Body Sweep unlock logic
- Adaptive mode suggestions
- 7-day / 14-day programs

### v0.5 (Weeks 16-18): Polish & PWA
- Custom font integration
- Dark mode
- React Native Web (PWA build)

### v1.0 (Weeks 19-20): Launch Preparation
- Play Store submission
- Marketing assets
- User documentation
- Public launch

---

## Risk Mitigation

### Technical Risks

| Risk | Mitigation |
|------|------------|
| Skia performance issues | Fallback to Reanimated if < 60fps |
| Audio latency on low-end devices | Test on multiple devices early |
| AsyncStorage limits | Monitor data size, plan SQLite migration |
| Expo build failures | Test EAS builds early in Week 1 |

### Timeline Risks

| Risk | Mitigation |
|------|------------|
| Week 1-2 runs over | Cut non-critical polish (e.g., haptics) |
| Alpha testers don't engage | Have backup tester pool ready |
| Critical bug discovered in Week 5 | Extend alpha by 3-5 days if needed |

### User Experience Risks

| Risk | Mitigation |
|------|------------|
| Breathing rhythm feels wrong | Make timing configurable (hidden setting) |
| Delta not meaningful to users | Add explainer text / tooltip |
| Users don't return for session 2 | Add gentle push notification (opt-in) |

---

## Development Workflow

### Daily Standups (Async)
- What did I complete yesterday?
- What am I working on today?
- Any blockers?

### Weekly Reviews
- End of each week: Review progress vs. roadmap
- Adjust next week's tasks based on learnings
- Document decisions in ADRs (Architecture Decision Records)

### Testing Strategy
- **Unit tests:** Critical logic (delta calculation, storage)
- **Component tests:** Slider, breathing circle
- **E2E tests (future):** Full session flow with Detox
- **Manual testing:** Daily on real device

---

## Handoff to Beta (Week 6+)

**Requirements for Beta:**
- ✅ All MVP features complete and tested
- ✅ No critical bugs
- ✅ Alpha feedback incorporated
- ✅ Performance targets met
- ✅ Basic analytics in place (session counts, crash reports)

**Beta goals:**
- 50 users
- 2-week testing period
- Refined UX based on larger dataset
- Prepare for Play Store soft launch
