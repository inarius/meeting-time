# Implementation Plan: Live UI Scaling & Presentation Prototyping Engine

Provide a live, multi-variable prototyping panel inside the Developer Debug modal (`⚙️ Debug`) so you can experiment with scaling, column width ratios, typography hierarchies, and management view continuity on actual hardware (phones, tablets, desktop, HDMI projector) in real-time, without requiring code edits or redeployments.

## User Review Required

> [!IMPORTANT]
> - **Zero Baseline Risk**: The default state will always match your exact existing styles (Preset A: "Baseline / Current"). All prototyping settings will be additive overlays controlled via CSS variables.
> - **Session Persistence**: Prototype settings will be saved in `sessionStorage` so refreshing the browser, toggling between views, or rotating your device during testing will preserve your prototype adjustments. A one-click "Reset to Baseline" button will immediately restore default production styles.
> - **Copy Settings Feature**: A "Copy Values" button in the debug modal will copy your exact winning numbers to your clipboard so you can paste them to lock them in permanently.

---

## Proposed Changes

### Core CSS & Layout Tokens

#### [MODIFY] index.html

1. **Introduce Prototyping CSS Variables on `:root`**:
   - `--proto-scale`: Global presentation scale multiplier (default: `1.0`).
   - `--proto-left-col`: Left column width for presentation view (default: `35%`).
   - `--proto-title-scale`: Title size multiplier (default: `1.0`).
   - `--proto-details-scale`: Details body font multiplier (default: `1.0`).
   - `--proto-nextup-scale`: Next Up card font/padding scale (default: `1.0`).
   - `--proto-mgmt-continuity`: Boolean/class switch to align management view fonts, card borders, and timeline spacing with presentation design tokens.

2. **Refactor Selector Sizing to Consume Prototyping Variables**:
   - `.presentation-grid`: `grid-template-columns: minmax(260px, var(--proto-left-col, 35%)) 1fr;`
   - Typography clamps (`.current-title`, `.details-viewport`, `.meeting-title-bar`, `.next-up-card`): Scale cleanly with `calc([base-clamp] * var(--proto-scale, 1) * var(--proto-[element]-scale, 1))`.
   - When Preset is `Baseline`, all multipliers are `1.0` and left column is `35%`, behaving 100% identically to current code.

3. **Dynamic Font-Size Pagination Calculation**:
   - In `renderPresentationTextPages()`, replace the hardcoded `font-size: 1.25rem` measurement dummy element with `getComputedStyle(c).fontSize` and `lineHeight` so text page breaks always accurately reflect the active font size.
   - Hook prototype slider changes to re-trigger `renderPresentationTextPages()` so pagination flips adjust instantly.

4. **Expanded Debug Modal (`⚙️ Debug`) UI**:
   - **Preset Selection Dropdown**:
     - *Preset A: Baseline (Current)* — Current hand-tuned rules.
     - *Preset B: Harmonic Modular* — Proportional type scale where all text scales in mathematical lockstep.
     - *Preset C: Projector Boost* — Larger body details, bolder title emphasis, optimized for distance viewing in lit rooms.
     - *Preset D: Compact / Wide* — Wider details column (30% left / 70% right), compact header padding.
   - **Presentation Sliders**:
     - *Overall Presentation Scale*: 80% to 130% range.
     - *Left Column Width*: 25% to 45% range (default 35%).
     - *Title vs Details Emphasis*: Slider to boost or reduce current title relative to body notes.
   - **Separate Management View Setting**:
     - *Checkbox: "Management View Continuity"* — Harmonizes editor article borders, input heights, and timeline row typography with presentation tokens.
   - **Action Bar**:
     - *Reset to Baseline*: Clears prototype overrides.
     - *Copy Settings*: Copies chosen numbers (e.g. `{ preset: "Harmonic", scale: 1.1, leftCol: "32%", mgmtContinuity: true }`) to clipboard.

---

## Verification Plan

### Automated Tests / Local Checks
- Run syntax and HTML validation to ensure no unclosed tags or Alpine syntax errors.
- Test in headless Chromium using Puppeteer to verify that:
  - Default load produces exact baseline font sizes and column widths.
  - Selecting presets updates `--proto-*` CSS variables on `:root`.
  - Re-rendering pagination recalculates without errors.
  - Resetting returns all CSS variables to initial defaults.

### Manual Verification on Devices
1. Push to GitHub Pages.
2. Open on phone in portrait (verify management view and portrait presentation lock).
3. Rotate phone to landscape (verify presentation view column widths and title sizing).
4. Open `⚙️ Debug`, test changing presets and moving the column width and scale sliders.
5. Verify page pagination updates cleanly with no clipped text.
