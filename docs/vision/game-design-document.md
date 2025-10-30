# Susokkan Focus — Mobile Game GDD v0.3

**Platform:** Android-first (Expo React Native → PWA via RN Web).
**Session length:** 2–10 minutes.
**Audience:** Anyone seeking calm alertness, mindful fun, and focus through traditional-inspired micro‑games.

---

## 1) Vision & Design Principles

* **Rooted in tradition, built for daily play.** Modern presentation of ancient attention practices (susokkan, gong listening, walking meditation, mindful breathing).
* **Meet the mind where it is.** Users self‑assess scatteredness → system suggests or adapts game modes.
* **Choice and agency.** Users can always pick their preferred mini‑game (Walking, Gong, Number Bubbles, Counting Ladder, or Balloon Breathing). Some modes unlock only when stability increases.
* **Game as mirror of mind.** The gameplay rhythm, precision, and feedback reflect the user's internal state.

---

## 2) Player Journey

1. **Pre‑Session Check‑In:**

   * Slider 0–10: *"How scattered or calm is your mind?"*
   * Label range (0–2 = calm, 3–6 = somewhat scattered, 7–10 = very scattered).
   * Choose intent: *Relax / Focus / Explore*.

2. **Mode Selection:**

   * All players can choose:

     * **Walking Meditation** (movement + counting)
     * **Gong Listening** (auditory focus)
     * **Number Bubbles** (visual counting 1–10)
     * **Counting Ladder** (susokkan exhale rhythm)
     * **Balloon Breathing** (belly breathing synch)
   * **Body Sweep** unlocks when: average scatteredness ≤ 3 OR consistent calm feedback across 3 sessions.

3. **Gameplay:** 2–8 minute mini‑session.

4. **Post‑Game Reflection:**

   * Slider: *"How stable and calm do you feel now?"*
   * Compare pre/post difference.
   * Optional note: *"What did you notice?"*

5. **Adaptive Suggestion:** Based on improvement or regression, suggest next suitable mode (e.g., "Try Gong Listening next time to deepen calm.")

---

## 3) Core Modes (Selectable)

### A) **Walking Meditation**

* Alternate taps for left/right steps.
* Count internally 1→10 per foot, then switch.
* Visual: simple footpath; footsteps appear; gentle environmental audio.
* Feedback: rhythm stability, smoothness.

### B) **Gong Listening**

* Hold while sound audible; release when it fades.
* For scattered mind, multiple gongs randomly strike (requires alertness to choose the active one).
* Later levels: single gong, longer fade (refined hearing).

### C) **Number Bubbles (1→10 Loop)**

* Tap floating bubbles 1→10; resets to 1 on error.
* Clean streaks make visuals slower, calmer.

### D) **Counting Ladder (Susokkan)**

* Tap stepping stones labeled 1–10, one per exhale.
* Wrong step = gentle tone, reset to previous.
* Optional breath‑guide circle (expand/contract).

### E) **Balloon Breathing (Belly Expansion)**

* Inhale → balloon expands; exhale → contracts.
* Slide/tap to match rhythm visually.
* Sounds at full inhale/exhale reinforce timing.
* Smoothness = score.

### F) **Body Sweep (Unlockable)**

* Glowing line moves slowly down the screen.
* Player observes bodily sensations as it moves.
* No score, only duration and presence tracking.

---

## 4) Adaptive Flow & Feedback

* Before and after every game, the user self‑rates scatteredness.
* If improvement ≥ 2 points → suggest calmer mode next time.
* If same or worse → suggest more engaging mode.
* App keeps a rolling average of *"mind stability delta."*
* Data visualized subtly (weekly stability trend).

---

## 5) Progression & Rewards

* **Unlocked by calm consistency:**

  * Body Sweep unlocks after 3 sessions with post‑score ≤ 3.
* **Seals:** "Steady Steps," "Silent Gong," "Clean Tens," "Calm Breath," "Whole Body."
* **Programs:** 7‑Day Focus Path, 14‑Day Calm Builder, Anytime Micro‑Reset.

---

## 6) Interface & Flow

**Home Screen:**

* Top: "How are you feeling?" slider.
* Center: Mode selection (cards with gentle animation).
* Bottom: Streaks, last session delta (+3 calmer, etc.).

**In‑Game:**

* Minimal UI; clean progress cues.
* Soft audio feedback.

**Post‑Game:**

* Calmness slider → reflection.
* Graph comparing pre/post stability.
* Suggest next mode.

---

## 7) Adaptive Engine

* Inputs: errors, rhythm, tap variance, session length, pre/post calm difference.
* Outputs: suggested mode, visual intensity, pace.
* Long‑term: 30‑day rolling trend of self‑reported stability.

---

## 8) Aesthetics & Audio

* Visual palette adapts to calmness (more muted as stability increases).
* Soundscapes: forest, river, temple, ambient drones.
* Haptic feedback minimal, reinforcing success not failure.

---

## 9) Tech Stack

* **Framework:** Expo RN + Web
* **Graphics:** react-native-skia
* **Audio:** expo-av
* **Haptics:** expo-haptics
* **State:** zustand / redux-toolkit
* **Data:** AsyncStorage + SQLite for logs

---

## 10) Roadmap

* **Week 1:** Core UI + Pre/Post check-in system.
* **Week 2:** Walking + Balloon prototypes.
* **Week 3:** Gong + Number Bubbles.
* **Week 4:** Counting Ladder + Body Sweep unlock logic.
* **Week 5:** Adaptive engine + progression tracking.
* **Week 6:** UI polish + translation.
* **Weeks 7–8:** Closed alpha (Bali/VN test group).
* **Weeks 9–10:** Beta + Play Store + PWA.

---

## 11) Long-Term Expansion

* Mic-based breath detection (privacy-safe).
* Wearable sync (step sensors for walking mode).
* "Guided Sequence Mode" (Walking → Count → Breath → Stillness in one flow).

---

### Summary

The user always begins and ends each session by rating scatteredness. All mini‑games are available by choice, but **Body Sweep** unlocks only once calm stability is demonstrated. Adaptive suggestions, gentle reflection, and visual/audio softening ensure the experience evolves as attention deepens.
