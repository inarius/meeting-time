# Implementation Plan: Google Cast via W3C Presentation API (Option C)

Add independent casting capability to `meeting-time` using the standard W3C Presentation API. A target TV / Chromecast loads the presentation URL (`?view=presentation&autoplay=1#agenda=...`) and runs autonomously on its own internal clock. The phone can disconnect without interrupting the presentation, re-establish connection to check status, push updated agendas, or terminate the cast.

## User Review Required

> [!IMPORTANT]
> - **Minimal Option C (Autonomous Receiver)**: The TV runs on its own local wall clock and deterministic agenda timestamps. This avoids complex bidirectional WebSocket/WebRTC messaging, eliminating lag, race conditions, and battery drain.
> - **Button Labels & Icons**: As requested, the button will be strictly labeled **"Cast"** when idle/disconnected, and **"Casting"** when active, both paired with the official Google Cast SVG icon.
> - **Connection Lifecycle**:
>   - `connection.close()` disconnects the phone controller while leaving the TV running undisturbed.
>   - `connection.terminate()` actively commands the TV receiver to exit and close the presentation.
>   - `request.reconnect(savedId)` detects if the TV is still alive and re-attaches the phone controller.
> - **Historical Plan Archival**: Antigravity updates `implementation_plan.md` in-place by default. As part of this change, we will create a dedicated `docs/plans/` folder in the repository and archive past plans (starting with `docs/plans/2026-09-10-live-ui-scaling-prototyping.md`) so they are permanently preserved and version-controlled.

---

## Proposed Changes

### Archival & Documentation

#### [NEW] [docs/plans/2026-09-10-live-ui-scaling-prototyping.md](file:///workspaces/meeting-time/docs/plans/2026-09-10-live-ui-scaling-prototyping.md)
- Archive the previous Live UI Scaling & Presentation Prototyping Engine implementation plan into the Git repository.

---

### Core Application & Casting Engine

#### [MODIFY] [index.html](file:///workspaces/meeting-time/index.html)

1. **URL Parameter & Kiosk Receiver Support**:
   - In `initApp()`, inspect URL search parameters:
     - `?autoplay=1`: Automatically calls `this.resumeAuto()` on load so a receiver immediately begins displaying and advancing topics without requiring physical tap interaction.
     - `?view=presentation`: Sets a `isPresentationOnly` flag that hides the management view section entirely or locks scroll to 0, ensuring a clean kiosk display on TV/Chromecast devices.

2. **W3C Presentation API Controller**:
   - Add reactive properties to `meetingApp()`:
     - `isCastSupported`: Detects `window.PresentationRequest` and `navigator.presentation`.
     - `isCastAvailable`: Tracks receiver availability via `request.getAvailability()`.
     - `castConnection`: Holds the active `PresentationConnection` instance.
     - `isCasting`: Computed / getter (`castConnection && castConnection.state === 'connected'`).
     - `isReconnectingCast`: Boolean indicator during reconnect attempts.
   - Implement controller methods:
     - `initPresentationApi()`: Instantiates `PresentationRequest` pointing to `getCastURL()`, monitors availability events, and checks `localStorage` for an existing presentation ID to attempt background reconnect with `request.reconnect(savedId)`.
     - `getCastURL()`: Builds `https://<origin><pathname>?view=presentation&autoplay=1#agenda=<compressed>`.
     - `startCast()`: User gesture handler calling `presentationRequest.start()`. On success, stores `connection.id`, attaches state change handlers (`onclose`, `onterminate`), and updates button state.
     - `stopCast()`: Calls `connection.terminate()`, clearing local storage and dismissing the presentation on the TV.
     - `disconnectPhone()`: Calls `connection.close()`, disconnecting the phone while leaving the TV running.
     - `pushAgendaToCast()`: Reconnects or restarts presentation with current updated URL hash so the TV updates without having to manually set up the cast again.

3. **Top Navigation UI Updates**:
   - In `.top-nav-row` alongside "🔗 Share" and "📥 Import / Edit Raw":
     - When idle: Render `<button class="secondary" @click="handleCastButtonClick()">` with text **"Cast"** and Google Cast SVG icon.
     - When active: Render `<button class="primary" @click="handleCastButtonClick()">` with text **"Casting"** and active Google Cast SVG icon.
   - If casting is not supported by the browser (e.g. Firefox/Safari mobile without flags), button will hide or show an informational notice when tapped.

4. **Cast Management Modal**:
   - Add `<dialog x-ref="castModal">`:
     - Clean, focused dialog opened when clicking **"Casting"**:
       - Status header: *"Casting to Smart Screen / TV"*
       - Action 1: **Stop Casting** (terminates presentation on TV).
       - Action 2: **Disconnect Phone** (keeps TV presentation running, frees phone).
       - Action 3: **Push Updated Agenda** (updates TV with latest edits made on phone).
       - Action 4: **Close Menu** (dismiss dialog).

---

## Verification Plan

### Automated Verification
- Headless Puppeteer tests:
  1. Load page with `?view=presentation&autoplay=1` and verify that `isPlaying === true`, presentation view is visible, and the system clock and topic timers tick forward.
  2. Verify that top navigation contains the "Cast" button and that it has the Google Cast SVG icon.
  3. Mock `window.PresentationRequest` in the headless browser to verify:
     - `startCast()` successfully transitions button to **"Casting"**.
     - Clicking **"Casting"** opens the cast modal.
     - Calling `stopCast()` calls `terminate()` and resets state.
     - Calling `disconnectPhone()` calls `close()`.

### Manual Device Verification
1. Push to GitHub Pages.
2. Open on Chrome on Android or desktop Chrome connected to the same Wi-Fi as a Chromecast or Google TV.
3. Tap **"Cast"**: Verify Google Cast device picker opens and the TV launches `meeting-time`.
4. Verify the TV displays the presentation view and auto-advances topics in real-time.
5. Lock the phone or disconnect Wi-Fi on the phone: Verify the TV keeps running smoothly without pause.
6. Re-open the phone: Verify tapping **"Casting"** shows the dialog, and tapping **Stop Casting** successfully shuts down the TV display.
