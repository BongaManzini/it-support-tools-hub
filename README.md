# IT Support Tools Hub – Ticket Auto-Closer & Macro Engine

A Chrome Extension (Manifest V3) built for IT support agents that automates repetitive ticket-closing workflows and bulk account unlocking operations directly inside a service desk and Telviva VoIP portal — without any external dependencies.

---

## Overview

IT Support Tools Hub is an integrated browser-side automation workspace. It sits in Chrome's **Side Panel** and lets support agents record mouse-click sequences once, then replay them automatically across dozens of tickets or Telviva extension accounts. The extension injects a content script into the target pages to execute actions, observe state, and relay progress back to the panel UI in real time.

---

## Features

### Ticket Auto-Closer Module
- **Click Recorder** – Records mouse clicks and dropdown selections on any service desk page and saves them as replayable macro steps.
- **Auto-Run Loop** – Replays recorded steps across consecutive tickets automatically, navigating to the next ticket after each run.
- **Excel Paste Routing** – Paste rows directly from a spreadsheet; the engine extracts ticket numbers and maps them to the correct dropdown category (e.g. *Audio Drop – Agent Side / Caller Side / Both Sides*).
- **Category Dropdown Filter Tree** – Scrapes all `<select>` options from the live page and presents them in an accordion tree. Uncheck any option to permanently block it from being selected during automation.
- **Configurable Speed & Limits** – Set the maximum number of tickets to process, the pause delay between actions (ms), and a pop-up countdown timer.
- **Audit History Log** – Scrollable ledger of every closed ticket, the assigned agent name, and the dropdown option that was applied.
- **Pause / Fix Single Step / Duplicate Step** – Fine-grained control over recorded macro sequences without re-recording from scratch.

### Account Unlocker & Password Reset Module (Telviva)
- **Batch Unlock Loop** – Iterates through a list of Telviva extension numbers, navigating the phone-list table (including multi-page pagination) and opening each account's edit screen automatically.
- **Auto-HTML Table Scanner** – If no account list is provided, the engine automatically scrapes all extension numbers visible on the current page.
- **Password Strategy Options**
  - *Dynamic Owner Mode* – Sets each account's password to its owner name as scraped from the page.
  - *Fixed Global Password* – Uses a single administrator-defined password for every account in the batch.
- **Lock Clearance** – Automatically detects and unchecks the "Locked" toggle for each account before saving.
- **Custom Unlock Macro Steps** – Record additional clicks on the edit page (e.g. opening a nested settings modal) that run before the password fields are populated.
- **Execution Process Monitor** – Displays the currently active extension target in real time.

### General UI
- **Chrome Side Panel** – Always-visible workspace that does not block the active tab.
- **Draggable Dashboard Cards** – Every section card within the panel can be reordered by drag-and-drop.
- **Three Visual Themes** – Midnight Slate (default), Nordic Frost, and Muted Olive; preference is persisted across sessions.
- **On-Screen Status Overlays** – Colour-coded banners injected directly into the target page show live automation state (Scanning, Saving, Finished, Error, etc.).
- **Recording Banner HUD** – A fixed red badge appears on the page whenever the click recorder is active.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Platform | Google Chrome Extension – Manifest V3 |
| Background | Service Worker (`scripts/background.js`) |
| Content Injection | Content Script (`scripts/content_script.js`) |
| UI | Vanilla HTML / CSS / JavaScript (`panel/panel.html`, `panel/panel.js`) |
| State Persistence | `chrome.storage.local` |
| Permissions | `storage`, `activeTab`, `scripting`, `sidePanel`, `tabs` |

**No build step. No npm. No external libraries.** The extension runs entirely on plain browser APIs.

---

## Supported Sites

| Site | Purpose |
|---|---|
| `https://servicedesk.alteram.co.za/*` | Ticket closing automation |
| `https://portal422.telviva.com/*` | Telviva account unlock & password reset |

---

## Installation

> [!IMPORTANT]
> This extension is loaded as an **unpacked extension** — it is not published to the Chrome Web Store.

1. Clone or download this repository to your local machine.
2. Open **Google Chrome** and navigate to `chrome://extensions`.
3. Enable **Developer Mode** (toggle in the top-right corner).
4. Click **"Load unpacked"**.
5. Select the root folder of this project (the folder containing `manifest.json`).
6. The extension icon will appear in your Chrome toolbar.

---

## Usage

### Opening the Panel
Click the **IT Support Tools Hub** icon in the Chrome toolbar. The side panel opens on the right side of the browser window.

### Ticket Auto-Closer
1. Navigate to the first ticket you want to close on `servicedesk.alteram.co.za`.
2. In the side panel, open the **Ticket Auto-Closer** tab.
3. Click **Record Clicks** and perform your closing sequence on the page (select dropdowns, click buttons, etc.). A red recording banner appears on the page.
4. Click **Stop** when done — your steps appear in the *Recorded Action List Sequence* section.
5. *(Optional)* Paste Excel rows into the **Copy and Paste Excel Rows Box** to enable per-ticket dropdown routing.
6. Set your **Maximum Ticket Runs Limit** and **Pause Time Between Actions**.
7. Click **Start Auto-Run**. The engine replays your steps on each ticket and advances automatically.

### Account Unlocker (Telviva)
1. Navigate to the Telviva phone list page (`/devices/phones/list`).
2. Open the **Account Unlocker Dashboard** tab.
3. *(Optional)* Paste a list of extension numbers (one per line) into **Target Account System IDs**. Leave blank to auto-scan the visible table.
4. Choose a **Password Reset Mapping** strategy.
5. *(Optional)* Click **Record Steps** to capture any extra clicks needed on the edit screen.
6. Click **Launch Accounts Batch Process Loop**. The engine navigates each account, resets the password, clears the lock flag, and saves.

### Tips
- Use **Fix Single Step** to re-record one specific action without clearing the whole sequence.
- Use **Duplicate** on a step card to repeat an action multiple times in a row.
- Click **Synchronize Dropdowns and Refresh System** to re-scrape the current page's dropdown options.
- Drag section cards by their grab-handle to rearrange the dashboard layout.

---

## Project Structure

```
Ticket Auto-Closer & Macro Engine/
├── manifest.json       # Extension manifest (MV3)
├── scripts/
│  ├── background.js     # Service worker – side panel launcher & message router
│  └── content_script.js   # Injected automation engine & DOM recorder
└── panel/
    ├── panel.html             # Side panel UI markup & styles
    └── panel.js               # Panel controller – state, events, rendering
```

---

## License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
