# Meeting Time

A visual meeting timer and agenda presentation designed for Scout Troop meetings, Patrol Leaders' Council (PLC), and board meetings. It displays cleanly on a projector or TV, keeping announcements, patrol rotations, and opening/closing milestones strictly on schedule without requiring anyone to stand by a computer clicking slides.

👉 **Launch the App:** [https://inarius.github.io/meeting-time/](https://inarius.github.io/meeting-time/)

---

## Quick Navigation
- [What You See on Screen](#what-you-see-on-screen)
  - [Top Screen: Presentation Display](#top-screen-presentation-display)
  - [Bottom Screen: Agenda Manager](#bottom-screen-agenda-manager)
  - [Key Visual Cards](#key-visual-cards)
- [How to Edit Your Agenda](#how-to-edit-your-agenda)
  - [Adding & Reordering Items](#adding--reordering-items)
  - [Formatting Notes & Bullets](#formatting-notes--bullets)
  - [Setting Up Patrols, Dates & Announcements](#setting-up-patrols-dates--announcements)
- [How to Share Your Agenda](#how-to-share-your-agenda)
  - [Method 1: Send a Web Link](#method-1-send-a-web-link)
  - [Method 2: Copy & Paste Plain Text](#method-2-copy--paste-plain-text)
- [Running the Meeting: Auto vs. Manual Control](#running-the-meeting-auto-vs-manual-control)
  - [Automatic Clock Timekeeping](#automatic-clock-timekeeping)
  - [The 1-Minute Wrap-Up Warning Pulse](#the-1-minute-wrap-up-warning-pulse)
  - [Manual Control & Automatic Resume](#manual-control--automatic-resume)
- [Casting to a TV or Projector](#casting-to-a-tv-or-projector)
- [Complete Button Cheat Sheet](#complete-button-cheat-sheet)

---

## What You See on Screen

The app is built as two full-screen views stacked on top of each other. You can effortlessly swipe or scroll vertically between them.

### Top Screen: Presentation Display
This is the main view projected to the room or cast to a TV.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│  TROOP 101 - TROOP MEETING                                            [ 07:15:23 PM ]  │
├───────────────────────────────────────────────────────────────────────┬────────────────┤
│                                                                       │  [P] PATROLS   │
│  NOW                                                                  ├────────────────┤
│  OPENING CEREMONY & FLAG OATH                                         │  [D] DATES     │
│  Scheduled: 07:00 PM                                                  ├────────────────┤
│                                                                       │  [A] ANNOUNCE  │
│  • Eagle Patrol post the colors                                       └────────────────┤
│  • Pledge of Allegiance, Scout Oath & Scout Law                                        │
│  • Senior Patrol Leader welcome remarks & inspection                                   │
│                                                                                        │
│  ┌─ NEXT UP ────────────────────────────────────────────────────────── 07:20 PM ─┐     │
│  │  Patrol Corner Breakouts & Skill Instruction                                  │     │
│  └───────────────────────────────────────────────────────────────────────────────┘     │
│                                                                                        │
│  [ ⏸ Pause ]  [ ◀ Prev ]  [ Next ▶ ]   [ ⚡ Cast ]   [ 🌓 ]  [ ⛶ ]   [ ▼ Edit Agenda ]  │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

### Bottom Screen: Agenda Manager
Scroll down (or tap `[ ▼ Edit Agenda ]`) to access the facilitator controls, timeline editor, and text importer.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│  ▲ Back to Presentation                                                                │
├───────────────────────────────────────────┬────────────────────────────────────────────┤
│  MEETING TITLE                            │  ITEM DETAILS & NOTES                      │
│  [ Troop 101 - Troop Meeting            ] │                                            │
│                                           │  [ Agenda Item ] [ Patrols ] [ Dates ] ... │
│  TIMELINE SCHEDULE                        │                                            │
│  ┌──────────────────────────────────────┐ │  Start Time: [ 07:00 PM ▼ ]                 │
│  │ ⠿  07:00 PM  Opening Ceremony    [✕] │ │  Title:      [ Opening Ceremony          ] │
│  │ ⠿  07:20 PM  Patrol Corners      [✕] │ │                                            │
│  │ ⠿  07:50 PM  Inter-Patrol Game   [✕] │ │  Meeting Notes (Bullets & Text):           │
│  │ ⠿  08:15 PM  Scoutmaster Minute  [✕] │ │  ┌──────────────────────────────────────┐  │
│  │ ⠿  08:25 PM  Closing & Dismissal [✕] │ │  │ - Eagle Patrol post the colors       │  │
│  └──────────────────────────────────────┘ │  │ - Pledge, Oath, Law                  │  │
│                                           │  │ -- Assigned: Johnny & Mark           │  │
│  [ + Add Item ]   [ 📋 Import / Edit Raw ] │  └──────────────────────────────────────┘  │
│  [ ↺ Undo ]       [ 🔗 Share ]  [ 🔄 Refresh ] │                                       │
└───────────────────────────────────────────┴────────────────────────────────────────────┘
```

---

### Key Visual Cards

#### 1. The "NOW" Milestone & System Clock
Shows the current active topic and the live wall-clock time:

```text
   NOW                                             ┌─────────────────┐
   OPENING CEREMONY                                │ [ 07:15:00 PM ] │  ◄ Real-World Clock
   Scheduled: 07:00 PM                             └─────────────────┘
```
- **Green Pill Clock**: Displays live seconds so the meeting leader always knows the exact real time.
- **Scheduled Time**: Shows when this specific part of the meeting was planned to start.

#### 2. The "Next Up" Milestone Card
Located right below the topic details, this card warns the room what is coming next:

```text
   ┌─ NEXT UP ────────────────────────────────────────────────────────── 07:20 PM ─┐
   │  Patrol Corner Breakouts & Skill Instruction                                  │
   └───────────────────────────────────────────────────────────────────────────────┘
```
- **Normal Mode**: Purple border showing the upcoming topic and start time. Clicking this card immediately advances to that topic.
- **1-Minute Pulse Mode (Approaching)**: Exactly **60 seconds** before the topic is scheduled to end, this card **gently pulses in size** to silently alert the speaker to wrap up!
- **First-Slide Alert Mode**: Tapping previous when on the very first slide turns this card orange and pulses continuously to grab attention before the meeting starts.

#### 3. The Rotating Side Tabs
Anchored along the right-hand edge are three colored vertical tabs:

```text
   ┌──────────────┐   • Green  [P] Patrols       (Rosters & assignments)
   │  [P] Patrols │   • Blue   [D] Dates         (Upcoming campouts & deadlines)
   ├──────────────┤   • Orange [A] Announcements (General troop reminders)
   │  [D] Dates   │
   ├──────────────┤
   │  [A] Announce│
   └──────────────┘
```
- While the meeting runs, the screen periodically slides out these reference cards for a few moments, then slides back to the main agenda.
- **Pre-Slide Bounce**: Three seconds before a tab slides out, the button physically bounces to catch your eye.
- **Manual Tap**: Tap any tab button to open it immediately; tap **Agenda** to collapse it back.

---

## How to Edit Your Agenda

### Adding & Reordering Items
1. **Scroll down** to the Management View (or click `[ ▼ Edit Agenda ]`).
2. **Add an item**: Click `[ + Add Item ]`. A new item appears on the timeline schedule.
3. **Change the time**: Select the item from the timeline, click the **Start Time** dropdown, and choose or type a time (e.g., `07:15 PM`).
4. **Change the title**: Type into the **Title** box.
5. **Reorder items**: Press and hold the grip icon `[ ⠿ ]` next to any timeline item and drag it up or down.
6. **Delete an item**: Click the `[ ✕ ]` button beside the item.
7. **Accidental deletion?** Just click `[ ↺ Undo ]` in the toolbar!

### Formatting Notes & Bullets
In the **Meeting Notes** text box, you can format notes using standard bullet points:

```text
- First main bullet point
-- Indented sub-bullet point
-- Another sub-bullet point
**Bold text** for critical reminders
*Italic text* for names or roles
```

> **Pro Tip for Long Notes:** If you type a lot of notes for a single topic, the app automatically splits the text into clean pages and flips between them like a slideshow every few seconds.

### Setting Up Patrols, Dates & Announcements
At the top of the details editor, click on the side-card buttons (**Patrols**, **Dates**, or **Announcements**):
- **Enable / Disable**: Check or uncheck the **"Include in presentation rotation"** box. Disabled cards are hidden and will not interrupt the presentation.
- **Edit Content**: Type your patrol lists, campout dates, or merit badge notices into the text area.

---

## How to Share Your Agenda

You can distribute the agenda to scouts, scribes, and adult leaders in two ways:

```text
               ┌────────────────────────────────────────────────────────┐
               │              HOW WOULD YOU LIKE TO SHARE?              │
               └───────────┬────────────────────────────────┬───────────┘
                           │                                │
                           ▼                                ▼
            ┌─────────────────────────────┐  ┌─────────────────────────────┐
            │   METHOD 1: SEND A LINK     │  │   METHOD 2: COPY RAW TEXT   │
            │     Click  [ 🔗 Share ]     │  │  Click [ 📋 Import / Edit ] │
            └──────────────┬──────────────┘  └──────────────┬──────────────┘
                           ▼                                ▼
            • Generates a single web link    • Plain text you can email,
            • Works on any phone or laptop     text, or print out
            • Entire meeting is saved        • Reusable template for next
              directly inside the link!        week's meeting
```

### Method 1: Send a Web Link
1. Click the `[ 🔗 Share ]` button in the management toolbar.
2. The complete meeting link is automatically copied to your clipboard.
3. Paste it into an email, group chat (Slack, Discord, WhatsApp), or text message.
4. When someone clicks the link on their phone, tablet, or computer, it opens the exact agenda you prepared. **No account, login, or cloud setup required!**

### Method 2: Copy & Paste Plain Text
If you prefer keeping a backup in Google Docs, printing a hard copy, or reusing last week's format:
1. Click `[ 📋 Import / Edit Raw ]`.
2. A window opens with the entire meeting formatted as simple readable text:

```text
07:00 PM Opening Ceremony & Flags
- Eagle patrol post colors
-- Johnny: Scout Oath
07:20 PM Patrol Corners
- Work on first aid skills
07:50 PM Inter-Patrol Knot Relay
08:15 PM Scoutmaster Minute
08:25 PM Closing Announcements & Dismissal

=== TABS ===
# Patrols [x]
- Eagles: Meeting Room A
- Cobras: Hallway North
# Dates [x]
- Oct 12-14: Fall Campout at Camp Wilderness
- Oct 22: Court of Honor
# Announcements [x]
- Annual dues due by end of month
```

3. **To Export:** Click `[ 📋 Copy to Clipboard ]` and paste it wherever you want.
4. **To Import:** Paste any agenda text into this box and click `[ 💾 Save & Apply Agenda ]`.

---

## Running the Meeting: Auto vs. Manual Control

### Automatic Clock Timekeeping
By default, the app is hands-off:
- The system checks the real-world clock every second.
- When the time reaches `07:20 PM`, the display automatically advances to the `07:20 PM` topic.
- Attendees can simply look at the screen to know where the troop should be.

### The 1-Minute Wrap-Up Warning Pulse
To keep the Senior Patrol Leader and instructors on schedule without awkward interruptions:
- **Exactly 60 seconds** before the current topic ends, the **Next Up** card begins **gently pulsing in size**.
- This provides a quiet, visual ambient cue across the room that it is time to conclude the current activity.
- The pulse activates whether the app is playing or paused, keeping everyone aware of real-world time.

### Manual Control & Automatic Resume
If a discussion runs over, or you want to inspect a previous item:
- **To Pause:** Tap `[ ⏸ Pause ]`, click `[ ◀ Prev ]` or `[ Next ▶ ]`, or click any item on the timeline.
- The button turns opaque and displays `[ ▶ Resume ]`. The screen will now stay on your selected topic indefinitely.
- **To Resume Automatically:** Simply navigate back to the item scheduled for the current clock time! The app recognizes you are back on track and **automatically unpauses**.
- You can also tap `[ ▶ Resume ]` at any time to resume auto-advancing.

---

## Casting to a TV or Projector

You can wirelessly cast the presentation from your mobile phone to a smart TV, Chromecast, or projector, allowing your phone to act as a wireless remote control.

```text
   ┌───────────────────┐    Google Cast / Smart TV    ┌───────────────────────────┐
   │  PHONE CONTROLLER │ ───────────────────────────► │   TV / PROJECTOR SCREEN   │
   │  (In Your Pocket) │                              │   Autonomous Presentation │
   └───────────────────┘                              └───────────────────────────┘
     • Screen can lock & sleep to save battery          • Runs continuously on clock
     • Unlocking phone silently reconnects              • Never falls asleep
```

### 1. Connecting to the TV
1. Ensure your phone and the TV/Chromecast are on the same Wi-Fi network.
2. On the presentation bar, tap `[ ⚡ Cast ]`.
3. Select your TV from the device list.
4. The TV launches the clean kiosk presentation (with buttons hidden), and your phone displays `[ 📡 Connected ]`.

### 2. The "Pocket Mode" Advantage
Unlike standard screen-mirroring, you do not have to leave your phone screen on:
- Once connected, **put your phone in your pocket and let the screen lock**.
- The TV runs **autonomously** on its own wall clock, advancing slides and cycling announcements automatically.
- Your phone's screen wake lock is released so your battery is not drained.

### 3. Reconnecting & Controlling
- When you need to pause or step through slides, pull out your phone and unlock it.
- The phone **silently reconnects in the background** within seconds, instantly mirroring the TV's current position.
- You can step forward or backward, change tabs, or pause the TV directly from your phone.

### 4. Disconnecting vs. Stopping
In the `[ ⚡ Cast ]` menu:
- **Disconnect Phone:** Detaches your phone controller without interrupting the meeting on the TV.
- **Stop Casting:** Shuts down the presentation on the TV screen.

---

## Complete Button Cheat Sheet

### Presentation View (Top Screen)

| Button | What It Looks Like | What It Does |
|---|:---:|---|
| **Play / Pause** | `[ ⏸ Pause ]` / `[ ▶ Resume ]` | Halts or resumes automatic wall-clock advancing. |
| **Previous Topic** | `[ ◀ Prev ]` | Steps backward to the previous agenda item (pauses timer). |
| **Next Topic** | `[ Next ▶ ]` | Steps forward to the next agenda item (pauses timer). |
| **Next Up Card** | `┌─ NEXT UP ─┐` | Tapping the Next Up card also advances directly to that topic. |
| **Wireless Cast** | `[ ⚡ Cast ]` / `[ 📡 Connected ]` | Opens the wireless TV connection menu. |
| **Theme Toggle** | `[ 🌓 ]` | Switches between **Dark Mode** (dim hall/OLED) and **Light Mode** (high-ambient projector). |
| **Fullscreen** | `[ ⛶ ]` | Expands the presentation to fill the entire monitor or projector. |
| **Edit Agenda** | `[ ▼ Edit Agenda ]` | Slides down to the timeline manager and editor. |
| **Side Tabs** | `[P]` `[D]` `[A]` | Tapping any tab opens that card; tapping **Agenda** closes it. |

### Management View (Bottom Screen)

| Button | What It Looks Like | What It Does |
|---|:---:|---|
| **Back to Top** | `[ ▲ Back to Presentation ]` | Slides back up to the big-screen presentation view. |
| **Add Item** | `[ + Add Item ]` | Inserts a new milestone item into the schedule. |
| **Import / Edit Raw** | `[ 📋 Import / Edit Raw ]` | Opens the plain-text editor to bulk paste or copy the agenda. |
| **Undo** | `[ ↺ Undo ]` | Reverts your last edit, item deletion, or timeline reorder. |
| **Share Link** | `[ 🔗 Share ]` | Copies a complete, compressed web link to your clipboard. |
| **Check Update** | `[ 🔄 Refresh ]` | Checks if a newer version of the app is available and refreshes. |
| **Drag Grip** | `[ ⠿ ]` | Press and drag up/down on any timeline item to change its order. |
| **Delete Item** | `[ ✕ ]` | Removes that specific item from the meeting schedule. |

---

## Tips for Troop Scribes & Senior Patrol Leaders

1. **Build the agenda before the meeting:** Fill out your schedule and side cards ahead of time, click `[ 🔗 Share ]`, and text the link to the adult leaders and patrol leaders.
2. **Keyboard Shortcuts:** If your laptop is plugged into the projector, you can press the **Left Arrow (◀)** and **Right Arrow (▶)** keys on your keyboard to flip through topics without touching the mouse.
3. **Save it to your Home Screen (PWA):** On iPhone (Safari > *Share* > *Add to Home Screen*) or Android (Chrome > *Install App*), the app installs like a native app and works completely offline!
