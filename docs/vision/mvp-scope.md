# MVP Scope Definition

## MVP Goal
Deliver a **single, complete mini-game experience** that demonstrates the core value proposition:
> Self-awareness + simple interaction = measurable calm

---

## MVP Features

### ✅ Included in MVP

#### 1. **Pre/Post Check-in System**
- **Pre-session slider:** "How scattered or calm is your mind?" (0-10)
- **Post-session slider:** "How stable and calm do you feel now?" (0-10)
- **Delta calculation:** Show improvement/change
- **Simple data storage:** AsyncStorage for session history

#### 2. **Balloon Breathing Mini-Game**
- **Visual:** Animated circle that expands (inhale) and contracts (exhale)
- **Interaction:** Tap/hold to sync with breath rhythm
- **Audio:** Subtle sound cues at full inhale/exhale
- **Duration:** 2-3 minute session
- **Feedback:** Smoothness score based on rhythm consistency
- **Why this mode?** Simplest to implement, clearest demonstration of core loop

#### 3. **Home Screen**
- Welcome message
- Pre-session check-in slider
- Single "Start Balloon Breathing" button
- Last session result (if exists)

#### 4. **Post-Game Screen**
- Post-session check-in slider
- Pre/Post comparison visualization
- Simple reflection prompt: "What did you notice?" (optional text input)
- "Play Again" button

#### 5. **Basic Data Persistence**
- Store session history (date, mode, pre/post scores, notes)
- Display last session stats on home screen

---

### ❌ Excluded from MVP (Future Releases)

- ❌ Other mini-games (Walking, Gong, Number Bubbles, Counting Ladder, Body Sweep)
- ❌ Adaptive engine / mode suggestions
- ❌ Progression system / unlocks
- ❌ Seals / achievements
- ❌ 7-day/14-day programs
- ❌ Weekly stability trends / analytics dashboard
- ❌ Multiple visual themes
- ❌ Audio customization
- ❌ Haptic feedback
- ❌ Settings panel

---

## MVP User Flow

```
1. Launch App
   ↓
2. Home Screen: "How scattered or calm is your mind?"
   → Slider (0-10)
   ↓
3. Tap "Start Balloon Breathing"
   ↓
4. Game Screen:
   - Breathing circle animation
   - Tap/hold to match rhythm
   - 2-3 minute session
   ↓
5. Post-Game Screen:
   - "How stable and calm do you feel now?" → Slider (0-10)
   - Show delta: "You feel +3 calmer!"
   - Optional: "What did you notice?" (text input)
   ↓
6. Tap "Play Again" (loops to step 2) or Return Home
```

---

## MVP Success Criteria

**Technical:**
- ✅ App launches without crashes
- ✅ Smooth animations (60fps breathing circle)
- ✅ Data persists across sessions
- ✅ Works on Android (tested on 2+ devices)

**User Experience:**
- ✅ Session completes in 2-3 minutes
- ✅ Pre/post delta is clearly visible
- ✅ Breathing rhythm feels natural and calm
- ✅ Audio cues enhance (not distract from) experience

**Validation:**
- ✅ 10 alpha testers complete 3 sessions each
- ✅ Average post-session score ≥ 2 points calmer than pre-session
- ✅ 80%+ report they would use it again

---

## Timeline Estimate

**Week 1:** Core UI + Pre/Post check-in system
**Week 2:** Balloon Breathing game implementation
**Week 3:** Polish, audio, data persistence
**Week 4:** Internal testing + iteration
**Week 5:** Alpha testing (10 users in Bali/VN)

**Target:** MVP ready for alpha in 5 weeks

---

## Post-MVP Roadmap

**v0.2:** Add Walking Meditation + Number Bubbles
**v0.3:** Add Gong Listening + Counting Ladder
**v0.4:** Unlock Body Sweep + adaptive suggestions
**v0.5:** 7-day programs + progression tracking
**v1.0:** Full 6-mode experience + Polish → Play Store
