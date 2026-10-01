# IDEAX 2026 — Projector Countdown Display

A full-screen, projector-ready countdown and schedule display for the **IDEAX 2026** hackathon (*Build • Create • Compete*). It shows a large animated countdown for the current block (sprint, meal, etc.), an animated robot mascot, and a separate control panel so organizers can run the event from a second screen.

## Features

- **Live countdown** with progress bar for each schedule block
- **Eight display states**: Welcome, Opening, Development (sprints), Breakfast, Lunch, Dinner, Announcement, Closing
- **Animated mascot** (GSAP) that reacts when clicked, plus transparent-background mascot videos: eating a burger during **Breakfast** and noodles during **Dinner** (Lunch keeps the original drawn animation)
- **Editable schedule**: add sprints, set durations, choose where they're inserted, or jump to a specific sprint number
- **Announcements**: push a title and message to the projector
- **Separate control panel** that syncs with the display in real time (`BroadcastChannel`)
- **State persistence** via `localStorage`, so a refresh doesn't lose the schedule or timer
- **Fullscreen** mode with screen wake lock
- No build step and no dependencies to install

## Getting started

### Requirements

- A modern browser (Chrome or Edge recommended)
- An internet connection on first load (Google Fonts and GSAP are loaded from CDNs)

No installation, server or build step is needed.

### Run

Double-click `index.html`. That's the projector display. To open the control panel, open the same file in a second tab or window of the same browser with `#control` added to the address:

| Window | Address |
| --- | --- |
| Projector display | `.../index.html` |
| Control panel | `.../index.html#control` |

Both windows must be in the **same browser** on the same computer, since syncing uses `BroadcastChannel` and `localStorage`, which are per-browser. If syncing doesn't work when opened from a `file://` path in your browser, serve the folder with any static host (for example the VS Code *Live Server* extension) and open both windows from that address.

## How to use the app, A to Z

### 1. Open the app
1. Keep all files in one folder (`index.html` plus the images).
2. Double-click `index.html` to open it in a modern browser such as Chrome or Edge.

### 2. Set up the two screens
1. **Projector screen:** with `index.html` open, drag that browser window to the projector.
2. Press **`F`** (or click **Fullscreen**) to go fullscreen. The screen is kept awake while fullscreen.
3. **Operator screen:** in the *same browser*, open a second tab or window with the same address and add `#control` to the end (for example `file:///.../index.html#control`). This is the full control panel.
4. On the projector window you can also press **`C`** (or click **⚙ CONTROL**) to slide the control panel over the display, or use the bottom quick-controls bar. Close it again with the **← Back to display** button at the top of the panel, with `Esc`, or with `C`. On the standalone control page (`#control`), the same button takes you back to the display page.

> Both windows must be in the same browser on the same computer. They sync through `BroadcastChannel` and `localStorage`, which don't work across different browsers or machines.

### 3. Pick the block to show
The event is a list of **blocks**: Welcome, Opening, Sprint 01, Breakfast, Sprint 02, Lunch, Sprint 03, Dinner, Final Sprint, Closing.

- Use **◀ / ▶** in the bottom bar (or the `←` / `→` keys) to move between blocks.
- In the control panel, use **◂ Back** and **Next block ▸** to step one block backwards or forwards.
- Use the dropdown next to them to jump straight to any block.
- In the control panel, click a state button (`welcome`, `opening`, `development`, `breakfast`, `lunch`, `dinner`, `announcement`, `closing`), or press keys `1`–`9` on the display to jump straight to the Nth block of the schedule (default order: `1` Welcome, `2` Opening, `3` Sprint 01, `4` Breakfast, `5` Sprint 02, `6` Lunch, `7` Sprint 03, `8` Dinner, `9` Final Sprint, `0` Closing). If you add sprints, the numbers follow the order shown in the schedule list.

Each state has its own look: the mascot changes for Opening, plays the burger video during Breakfast, the noodles video during Dinner, and eats with the drawn animation during Lunch.

### 4. Run the countdown
1. Select a timed block (a sprint or a meal). Its duration is loaded automatically.
2. Click **Start** (or press `Space`). The big timer and progress bar begin counting down.
3. Click **Pause** (or `Space` again) to stop, and **Reset** (or `R`) to restore the block's full duration.
4. Fix the time on the fly with **+5 min / −5 min** (or `↑` / `↓`).
5. To set an exact time, type `hh:mm:ss` (for example `01:30:00`) in the control panel's duration box and click **Set**. The **10s** button loads a 10-second test timer.
6. Turn on **Auto: on** in the control panel so the next block is selected automatically when a countdown ends. Leave it off to advance manually.

### 4b. The 48-hour event countdown
A small **48H EVENT** clock sits at the top centre of every screen, including Welcome.
- It shows `48:00:00` (dimmed) while you're on the Welcome page, and **starts automatically the first time you move on** from Welcome to any other block.
- Adjust it with the **48H −5 / 48H +5** buttons in the bottom bar, the **48H −5 min / +5 min** buttons in the control panel, or the `-` / `+` keys. Each press moves it by 5 minutes.
- **48H reset** (control panel) sets it back to `48:00:00` and waits for the next move away from Welcome.
- It's independent of the block timer, so it keeps running through every scene, pause and reset of the block countdown. Its state is saved, so a refresh doesn't restart it.

### 5. Add a sprint
Use whichever is faster for you:
- **Quick:** click **+ 1h Sprint**, **+ 2h Sprint** or **+ 3h Sprint** in the control panel's *Sprint & Schedule Manager*.
- **Full form:** click **⚡ + ADD SPRINT** on the display (or press `S`, or click **+ Sprint** in the bottom bar). Fill in:
  - **Sprint title**, for example `SPRINT 04`
  - **Subtitle / phase**, for example `DAY 2 · FINAL BUILD`
  - **Duration**: choose a preset (30m to 4h) or type a custom `HH:MM:SS`
  - **Insertion point**: *Before Closing* (recommended), *Immediately After Current Sprint*, or *At the End*
- Click **➕ ADD SPRINT** to add it to the schedule, or **🚀 ADD & JUMP NOW** to add it and switch to it immediately.
- The dialog lists the current schedule blocks so you can check the order.

### 6. Change the sprint number
Press `N`, click **🔢 SPRINT #**, or use the **SPRINT ◀ ▶** stepper in the bottom bar.
- Pick Sprint 1–5 quickly, or type a number from 1 to 99. A live preview shows the result.
- **✓ SET SPRINT NUMBER** only renames the current sprint. **🚀 SET & SWITCH** also switches to that sprint.
- In the bottom bar you can also type a number into the sprint box and press `Enter`.

### 7. Show an announcement
1. In the control panel, type a **Title** and a **Message** under *Announcement*.
2. Click **Display** to show it on the projector (or press `A` to show the last announcement, or a default "HELLO, BUILDERS" message if none was set).
3. Click **Clear** when you're done, then pick the next block.

### 8. Interact with the mascot
During the **Opening** state, click the robot on the projector and it reacts with an animation.

### 9. Close out the event
Move to **Closing** (press `0`, or use the dropdown or `▶`). Then close the browser windows when finished.

### 10. If something goes wrong
| Problem | Fix |
| --- | --- |
| Control panel doesn't affect the display | Make sure both are open in the same browser on the same computer and opened the same way (both by double-clicking `index.html`, or both through the same Live Server address). |
| Images don't load | Keep all image files in the same folder as `index.html`. |
| Fonts or animation missing | The first load needs internet access for Google Fonts and GSAP. |
| Timer or schedule looks wrong after a refresh | State is restored from `localStorage`. Use **Reset**, or clear this page's site data in the browser to start fresh. |
| Keyboard shortcuts do nothing | Click the page background first, since shortcuts are ignored while typing in a field. |

## Keyboard shortcuts and controls

The control panel exposes all actions. The display also accepts these keyboard shortcuts:

| Key | Action |
| --- | --- |
| `Space` | Start / pause the timer |
| `R` | Reset the current block |
| `←` / `→` | Previous / next block |
| `↑` / `↓` | +5 / −5 minutes |
| `1`–`9` | Jump to the 1st–9th schedule block (Welcome, Opening, Sprint 01, Breakfast, Sprint 02, Lunch, Sprint 03, Dinner, Final Sprint) |
| `0` | Jump to the last block (Closing) |
| `A` | Show the announcement |
| `-` | Take 5 minutes off the 48-hour event countdown |
| `+` (or `=`) | Add 5 minutes to the 48-hour event countdown |
| `S` | Open the *Add Sprint* dialog |
| `N` | Open the *Change Sprint Number* dialog |
| `C` | Toggle the control panel |
| `F` | Toggle fullscreen |
| `Esc` | Close dialogs and the control panel |

## Customizing the schedule

The default schedule is defined in the `SCHEDULE` constant near the top of the script in `index.html`:

```js
const SCHEDULE = {
  event: { name: 'IDEAX 2026', tagline: 'BUILD • CREATE • COMPETE' },
  blocks: [
    { state: 'development', label: 'SPRINT 01', sub: 'DAY 1 · BUILD', sec: 2 * 3600 },
    { state: 'lunch', label: 'LUNCH', sec: 3600 },
    // ...
  ]
};
```

Each block has a `state` (one of the display modes), a `label`, an optional `sub` line, and a duration `sec` in seconds. The current timings are placeholders, so adjust them to your event. Sprints can also be added at runtime from the UI.

## Project structure

```
.
├── index.html          # Display, control panel, styles and logic (single file)
├── burger.webm         # Breakfast mascot video (transparent background)
├── noodels.webm        # Dinner mascot video (transparent background)
├── burger.mp4, noodels.mp4  # Original source videos (not used by the page)
├── mascot-clean.png    # Mascot used on the display
└── mascot.jpg, robot.png, bg.jpg  # Additional artwork
```

## Replacing the Breakfast / Dinner videos

The page plays `burger.webm` and `noodels.webm`. An ordinary `.mp4` has no transparency, so a replacement must be a **WebM (VP9) file with an alpha channel**, with the background already removed. Keep the same file names, or change the `Robot.mountVideo('...')` calls in the `breakfast` and `dinner` scenes in `index.html`. Keep the `.webm` files in the same folder as `index.html`.

## Configuration

- **Branding**: edit the event name and tagline in `SCHEDULE`, and the HUD text in `index.html`.

## Tech stack

Plain HTML, CSS and vanilla JavaScript, with [GSAP](https://gsap.com/) (loaded from a CDN) for animation. There is no backend, build step or package install.
