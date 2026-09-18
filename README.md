# Meeting Time

A visual meeting timer and agenda display for Scout Troop meetings, Patrol Leaders' Council (PLC), and board meetings. It runs directly in the browser on a laptop, tablet, or phone, and can be projected or wirelessly cast to a TV to keep meetings running on time hands-free.

👉 **Launch the App:** [https://inarius.github.io/meeting-time/](https://inarius.github.io/meeting-time/)

---

## How It Works

The app consists of two screens stacked vertically:

- **Presentation View (Top Screen):** The clean big-screen display shown to the room. It displays the current topic, live system clock, notes, what's coming up next, and rotating announcements.
- **Agenda Manager (Bottom Screen):** Scroll down to view the timeline, edit topics, manage announcements, and share the schedule.

---

## Editing Your Agenda

Scroll down to the Agenda Manager to customize your meeting:

- **Select an Item:** Click any topic in the timeline list to load its details into the editor on the right.
- **Add an Item:** Click `[ + Add ]` to create a new agenda topic.
- **Change Time & Title:** Edit the **Time** and **Title** fields, then click `[ Save ]`.
- **Reordering Items:** Items are strictly sorted by time. To move a topic up or down in the schedule, simply adjust its scheduled start time.
- **Delete an Item:** Select the item and click `[ Delete ]`.
- **Formatting Notes:** In the **Detail Content** box:
  - Type `- ` for a bullet point.
  - Type `-- ` for an indented sub-bullet.
  - Use `**bold**` or `*italics*` for emphasis.
- **Side Cards (Patrols, Dates, Announcements):** Select **Patrols**, **Dates**, or **Announcements** in the list to update their notes, or toggle **Enabled in Rotation** to choose whether they appear during the meeting.

---

## Sharing an Agenda

There are two easy ways to distribute an agenda:

### 1. Send a Web Link (`[ 🔗 Share ]`)
Click `[ 🔗 Share ]` in the top toolbar to copy a link to your clipboard. 
- The entire meeting agenda is saved directly inside the link.
- Anyone opening the link sees the exact schedule you created.
- No accounts, logins, or cloud databases required.

### 2. Copy or Paste Plain Text (`[ 📋 Import / Edit ]`)
Click `[ 📋 Import / Edit ]` to open the full agenda as plain text:
- **Exporting:** Click `[ Copy to Clipboard ]` to paste the text into an email, Google Doc, or troop group chat.
- **Importing:** Paste an existing schedule into the box and click `[ Save & Apply Agenda ]` to load it instantly.

---

## Running the Meeting

### Automatic Mode (Default)
When playback is running (`[ ⏸ Pause ]` visible):
- The app monitors the real-world clock.
- When the time reaches the next scheduled item (e.g., `07:20 PM`), the presentation automatically advances.
- The screen intermittently cycles between the main agenda and enabled announcement cards.

### Manual Mode
- Tap `[ ⏸ Pause ]`, `[ ◀ Prev ]`, `[ Next ▶ ]`, or click any item on the timeline to take manual control.
- When paused, the presentation stays on your selected slide indefinitely.
- **Auto-Resume:** Navigating back to the topic scheduled for the current clock time automatically unpauses and resumes scheduled playback. You can also tap `[ Resume Auto ]` at any time.

---

## Casting to a TV or Projector

You can wirelessly send the presentation to a TV or projector while using your phone as a remote:

1. Tap `[ ⚡ Cast ]` and choose your Chromecast or smart TV.
2. The TV opens the full-screen presentation without edit controls.
3. **Pocket Mode:** You can lock your phone and put it in your pocket. The TV continues presenting independently on its own clock.
4. When you unlock your phone, it automatically reconnects so you can pause, advance slides, or check details.
5. To stop, tap `[ ⚡ Cast ]` and choose **Disconnect Phone** (leaves TV presenting) or **Stop Casting** (exits on TV).

---

## Button Cheat Sheet

| Button | Where to Find It | What It Does |
|---|---|---|
| `[ ⏸ Pause ]` / `[ ▶ Resume ]` | Navigation bar | Pauses or resumes automatic clock-based advancing. |
| `[ ◀ Prev ]` / `[ Next ▶ ]` | Navigation bar | Steps backward or forward one agenda item (pauses auto-advance). |
| `[ ⚡ Cast ]` | Navigation bar | Connects to a smart TV or Chromecast. |
| `[ ☀️ Light ]` / `[ 🌓 Dark ]` | Navigation bar | Toggles between Dark theme and Light theme. |
| `[ 📺 Full ]` | Navigation bar | Enters full-screen presentation mode. |
| `[ 📋 Import / Edit ]` | Navigation bar | Opens plain-text agenda importer and exporter. |
| `[ 🔗 Share ]` | Navigation bar | Copies the complete agenda web link to your clipboard. |
| `[ 🔄 Refresh ]` | Navigation bar | Checks for app updates. |
| `[ ↩ Undo ]` | Editor control bar | Reverts the last edit or deletion. |
| `[ Delete ]` | Editor control bar | Removes the currently selected agenda item. |
| `[ Save ]` | Editor control bar | Saves changes made to the current item or tab. |
| `[ + Add ]` | Editor control bar | Adds a new agenda topic. |
| `[ Resume Auto ]` | Timeline header | Unpauses and syncs back to the live clock. |
