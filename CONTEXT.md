# Project Context & AI Guidance: `meeting-time`

> **NOTICE FOR AI ASSISTANTS (Gemini, Claude, ChatGPT, Copilot, etc.):**  
> This document is the comprehensive ground-truth context and design intent for the `meeting-time` project. Read this document thoroughly before proposing code changes, answering questions, or refactoring. It captures the project history, hardware constraints, user philosophy, past AI regression pitfalls, and immutable design contracts.

---

## 1. Project Overview & URLs
- **Repository:** `inarius/meeting-time` (GitHub)
- **Live Deployment:** [https://inarius.github.io/meeting-time/](https://inarius.github.io/meeting-time/)
- **Core Principle:** **Zero-build, zero-server, single-file application**. The entire app runs directly in client browsers from a single [`index.html`](file:///workspaces/meeting-time/index.html) file hosted on GitHub Pages, accompanied only by standard PWA assets ([`site.webmanifest`](file:///workspaces/meeting-time/site.webmanifest) and app icons). No Node.js build steps, no webpack/vite bundles, no npm runtime dependencies, and no backend database.

---

## 2. Real-World Use Case & User Intent

### The Problem Being Solved
The app was created specifically for **Scout Troop meetings** (and board-meeting-style agendas) where youth leadership and adult advisors run structured, multi-topic schedules together. 

### Hardware & Environment Constraints
1. **Display & Lighting Environments:** Meetings happen in halls, classrooms, or dim boardrooms. **Dark mode is the default application theme** (providing high-contrast legibility, energy efficiency on OLED displays, and an ambient presentation glow), with a **Light mode toggle** available for high-ambient projector environments where maximum lumens are desired.
2. **Projector Displays (4:3 and 16:9):** The app is projected onto a large screen, typically driven by a mobile phone, tablet, or laptop plugged into an HDMI cable or wireless casting receiver.
3. **Passive Ambient Display:** During the meeting, attendees and leaders should not have to manually advance slides or fiddle with a computer. The screen operates hands-off:
   - Displays **"Now"**: The current agenda item, its scheduled time, system clock, and detailed markdown notes.
   - Displays **"Next Up"**: A prominent alert card showing what topic starts next to keep speakers on time.
   - **Side Cards (Tabs)**: Intermittently rotates in general troop reference cards (**Patrols**, **Dates**, and **Announcements**) without interrupting the flow of the meeting.

---

## 3. History of Pain Points & Preventing Regressions

Over hundreds of iterations, multiple AI coding sessions suffered from recurring regressions. **Any AI working on this codebase must understand these past pitfalls and avoid repeating them:**

### 1. Inadvertent Loss of Working Features (The Need for "App Contracts")
- In earlier iterations, LLMs rewriting `index.html` frequently deleted subtle logic (e.g., losing visible button borders, breaking wake lock retention, breaking swipe deadzones, or ruining aspect-ratio typography).
- **The Solution:** Lines 11–69 of [`index.html`](file:///workspaces/meeting-time/index.html#L11-L69) contain the formal **APP CONTRACTS**. These rules represent hard-fought requirements. **Never alter or remove code satisfying these rules without the user's explicit consent.**

### 2. Screen Wake Lock Regressions
- **Requirement:** The device screen **must never sleep** during a meeting.
- **The Trap:** AIs frequently attached `wakeLock.release()` to the "Pause" button, causing tablets to go dark when paused.
- **The Rule:** The Screen Wake Lock API (`navigator.wakeLock`) must be acquired upon play or user resume, **must persist while paused**, and must automatically re-acquire via `document.addEventListener('visibilitychange')` whenever the browser returns to the foreground.

### 3. Dual-View Snapping vs. Halfway Scrolling
- **Requirement:** The interface is strictly **two screens stacked vertically**:
  1. **Top Screen (Presentation View):** Projector presentation.
  2. **Bottom Screen (Management View):** Timeline editor, element controls, and bulk text import.
- **The Rule:** Uses CSS scroll snapping (`scroll-snap-type: y mandatory`). Vertical scroll locks to either view and must **never rest halfway between them**. Do NOT break `height: calc(var(--vh, 1vh) * 100)` or add root scrollbars.

### 4. Touch Gestures & Diagonal Swipe Conflicts
- **Requirement:** Horizontal swiping (◀ / ▶) on the presentation screen shifts previous/next agenda items.
- **The Trap:** Diagonal swiping while trying to scroll down to the Management View accidentally skipped agenda items.
- **The Rule:** Touch handlers have a minimum deadzone (25px) and an axis-determination check. If vertical movement exceeds horizontal movement, the touch locks to vertical scrolling and horizontal swipe gestures are suppressed.

### 5. Playback Auto-Advance vs. Manual Inspection
- **Requirement:**
  - Real-time time sync: When playing, the app checks the system clock against agenda item scheduled times and auto-advances at real-time milestones.
  - Intentional navigation (clicking a tab, swiping to another topic, clicking a timeline item) **pauses** auto-advancing.
  - Passive actions (viewing, scrolling down to manage) do **not** pause.
  - **Auto-Resume:** Navigating back to the currently scheduled timeline item **automatically unpauses** and resumes live auto-advance.

### 6. Rejecting External Storage & API Dependencies
- Pastebin, GitHub Gists, Google Keep, and external database APIs were evaluated and rejected. External APIs introduce API keys, rate limits, offline vulnerability, and service deprecation risks.
- **The Rule:** Lossless persistence uses **`localStorage`** for offline device saving, and **`CompressionStream('deflate-raw')`** to compress the entire meeting title, agenda items, and tab contents into the URL hash (`#agenda=...`). The entire meeting state travels within the link itself.

### 7. Bidirectional Import / Export
- The plain-text format exported in the **"Import / Edit Raw"** modal must be parseable back into the app without data loss.
- Format: `HH:MM AM/PM Topic Name` followed by `-` bullets, `--` sub-bullets, and `=== TABS ===` with `# Tab Name [x]` for side-cards.
- Destructive imports wipe the in-memory undo stack and require a confirmation dialog.

### 8. Casting & Dual Authority Remote Control (CAF & W3C Presentation API)
- **Dual Authority Model:**
  - **Playing / Unpaused State:** The TV presentation is autonomous and is the primary Source of Truth, driven by its local wall clock. Whenever it advances to a new agenda milestone, it broadcasts `RECEIVER_STATUS` to the phone controller so an awake phone immediately mirrors the active topic.
  - **Interaction / Paused State:** When an intentional user action occurs on the phone controller (tapping a side tab, clicking next/prev, or toggling pause), both the TV and phone transition to `isPlaying: false`. In this paused state, the phone controller is authoritative, and the TV presentation faithfully follows every manual navigation step.
  - **Unpausing / Resuming:** Resuming playback restores the TV presentation as the autonomous authority on wall-clock time.
- **Screen Wake Lock Delegation:** When actively casting, the phone explicitly bypasses/releases its screen wake lock so the mobile device can sleep normally in the user's pocket to conserve battery. The TV presentation receiver independently maintains its own keep-alive.
- **Non-Clobbering Status Exchange (`REQUEST_STATUS` & `RECEIVER_STATUS`):** When a sleeping or backgrounded controller wakes up, or when the user performs a pull-down refresh while casting, the phone controller sends `REQUEST_STATUS` to the TV. The TV responds with `RECEIVER_STATUS` containing its active index, play state, and active tab. This prevents stale controller state from resetting the live presentation.
- **Receiver Kiosk Experience:** In presentation-only / receiver view (`?view=presentation`), interactive controls (play/pause buttons, cast buttons, edit triggers) are stripped from the DOM. If paused by the controller, an unobtrusive non-interactive `⏸ Paused` status badge is displayed.
- **Disabled Tab DOM Exclusion:** Disabled/unchecked side tabs are completely excluded from the presentation DOM so they do not show buttons or interfere with automated card cycling on either device.

---

## 4. Technical Architecture & Tech Stack

### Libraries (Loaded via CDN)
- **CSS:** [Pico CSS v2](https://picocss.com/) (`https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css`) provides clean semantic CSS variables and dark/light mode foundations.
- **Reactivity & State:** [Alpine.js v3](https://alpinejs.dev/) (`https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js`) provides lightweight, declarative reactivity directly in HTML attributes (`x-data`, `x-bind`, `x-on`, `x-show`, `x-model`).

### Key Configuration Block: `APP_CONFIG`
Located at lines 96–121 in [`index.html`](file:///workspaces/meeting-time/index.html#L96-L121):
```javascript
const APP_CONFIG = {
    appearance: {
        defaultTheme: 'dark'     // Options: 'dark' (default) or 'light'
    },
    timing: {
        agendaDuration: 40,      // Seconds the main agenda is displayed
        cardDuration: 20,        // Seconds each sidecard tab is displayed
        debugDuration: 5,        // Seconds for fast cycle simulation mode
        bounceLeadTime: 3,       // Seconds before tab transition to trigger the "bounce/tug" animation
        carouselInterval: 8000,  // Milliseconds between flipping pages of long text
        refreshTimeout: 180000   // Milliseconds before auto-refresh check gives up
    },
    swipe: {
        deadzone: 25,            // Pixels to swipe before registering the action
        dragDistance: 70         // Pixels to swipe to complete the transition
    },
    storage: {
        agendaKey: 'boardAgenda',
        tabsKey: 'boardTabs',
        themeKey: 'boardTheme_v2',
        titleKey: 'boardMeetingTitle',
        castSessionKey: 'meeting_presentation_id'
    },
    cast: {
        appId: '1909D2D6',       // Google Cast SDK Developer Console Custom Web Receiver Application ID
        namespace: 'urn:x-cast:inarius.meetingtime',
        maxInactivity: 10800     // Seconds (3 hours) of inactivity before receiver shuts down
    },
    history: {
        maxItems: 20             // Undo history stack limit
    }
};
```

### Side Cards / Rotating Tabs Mechanics
- Three fixed tabs: **Patrols** (`#16a34a` green), **Dates** (`#2563eb` blue), and **Announcements** (`#ea580c` orange).
- **Deck-Dealing Animation:** Enabled side cards slide in from the right edge (`translateX(0)`), overlaying the main agenda, and slide back out when returning to the agenda.
- **Visual Cues:** Tab progress bars fill during the display cycle; a `@keyframes softTug` bounce triggers 3 seconds before transition as a visual notice.
- **Debug Panel:** Includes a "Fast Cycle Simulation" (5s mode) accessible via the discreet `⚙️ Debug` link in the Management View to quickly test tab rotations.

### Typography & Mobile Responsiveness
- **Landscape Scaling:** Text and UI scale proportionally via `clamp()` and `vh` units so projector displays (low or high res) remain legible.
- **Portrait Lock:** In portrait orientation (used when editing on a phone), typography decouples from `vh` to prevent oversized text from overflowing the viewport.
- **Fluid Lists:** List indents scale with font size so bullets never clip outside containers.
- **Mobile Viewport Fix (`--vh`):** A custom JS handler calculates real viewport height on resize and orientation shifts to counteract mobile browser UI address bars and Android PWA launch rendering races.

---

## 5. Live UI Sizing Prototyper & Active Default Config

To eliminate past trial-and-error around screen scaling, a **Live Sizing & Layout Prototyper** is embedded directly into the Developer Debug modal (`⚙️ Debug`).

### Current Active Default Settings
The app is currently configured with the **Harmonic Proportional** base profile and user-tuned scale overrides:
```json
{
  "preset": "harmonic",
  "scale": "100%",
  "leftCol": "35%",
  "titleScale": "95%",
  "detailsScale": "95%",
  "nextUpScale": "95%",
  "mgmtContinuity": true
}
```
- **CSS Custom Properties on `:root`**:
  - `--proto-scale`: `1.0` (Global scale factor)
  - `--proto-left-col`: `35%` (Left column width in landscape grid)
  - `--proto-title-scale`: `0.95` (Title heading size multiplier)
  - `--proto-details-scale`: `0.95` (Details body font multiplier)
  - `--proto-nextup-scale`: `0.95` (Next Up card font/size multiplier)
- **Management View Continuity (`body.proto-mgmt-continuity`)**: Active by default. Aligns Management View column width, card borders, and timeline row font sizing with the presentation styling.
- **Dynamic Text Pagination**: `renderPresentationTextPages()` uses computed style font sizing and line height to guarantee accurate page splits across all scales.
- **Further Prototyping on Mobile**: The user can open `⚙️ Debug` on their phone to adjust sliders, test in landscape/portrait, or tap `📋 Copy Config` to export updated values.

---

## 6. Guidelines for Future AI Assistance

When interacting with the user or modifying this codebase:
1. **Respect the App Contracts:** Review lines 11–69 of [`index.html`](file:///workspaces/meeting-time/index.html#L11-L69) before touching layout, state, or event handling.
2. **Be Surgical & Concise:** Explain the "why" behind changes before outputting code.
3. **Preserve Comments & Structure:** Do NOT strip out configuration blocks, inline comments, or contract definitions.
4. **Never Force External Dependencies:** Do not recommend npm packages, node servers, or backend databases unless explicitly requested. Everything must remain self-contained in static client-side files.
5. **Acknowledge Mobile Vibe Coding:** The user frequently codes from mobile devices and phone browsers. Keep solutions practical, clean, and easily testable on GitHub Pages.

