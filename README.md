# 🧠 Nudge: The Conscious Productivity Tracker

**Nudge** is a tiny, always-on-top desktop app that measures your **real focus**, not just elapsed time. You tell it when you get distracted and why. It never guesses and never watches you.

> **Status:** in design. No release yet. The goal of v1 is a downloadable Windows installer (`.exe`) on the [Releases](https://github.com/CrisLottz/Nudge/releases) page.

---

## 🛑 The Problem

You block out 2 hours for deep work. During those 2 hours you check your phone, open a non-work tab, get up for a snack, or someone stops by to chat. When you look back, the 2-hour session held maybe 25 minutes of real, focused progress.

Most trackers either log the clock (total time) or try to guess whether you were "working" by watching your screen. Neither tells you the truth about your **intent**.

## ✅ The Solution: Conscious Accountability

Nudge runs two timers in parallel:

- **Total:** starts with the session and keeps running no matter what.
- **Distracted:** runs only while you are interrupted. You start it on purpose by pressing **Interrupt Work** (or a global hotkey) and picking a reason, like *Check phone*. You stop it with **Back to Work**.

At the end you get an honest number:

> **Real focus = Total time − Distracted time**

Admitting a distraction is the point. That small, deliberate action is what builds awareness.

### Not a surveillance tool

Nudge is the opposite of employee-monitoring software. It takes no screenshots, does no activity scoring, and needs no account or cloud. Everything is self-reported and stays in a local SQLite file on your machine.

---

## ✨ Features

### v1 (MVP)

- **📂 Projects:** organize sessions by project. Each project shows lifetime *Total*, *Distracted* and *Real* time.
- **⏱️ Session timer:** Play / Pause, **Interrupt Work** with a reason, and End Session.
- **⌨️ Global hotkey:** interrupt and resume without reaching for the mouse.
- **📌 Compact always-on-top mode:** a small strip (~320×110) with the timers and the interrupt button that sits above every window and expands to the full view on demand.
- **📜 Session log:** every session with its totals, plus a breakdown of time per distraction reason, so you can see what really breaks your focus.
- **🏷️ Custom distraction reasons:** defaults are *Check phone* and *Other*. Add, rename or delete your own.
- **💾 Export to JSON:** back up all your data in one file.

### Later

- 🌍 English / Spanish interface
- 💡 "Don't forget" reminders (up to 3, e.g. *Put the phone away*, *Drink water*)
- 🎨 Pixel-art theme
- 📥 Import from JSON
- 🔔 System tray icon

---

## 🛠️ Tech Stack

| Layer | Technology | Why |
|---|---|---|
| Desktop shell | [Tauri 2](https://tauri.app) | Small installers (a few MB, not 100+), plus official plugins for always-on-top windows, global shortcuts, SQLite, tray and auto-update |
| Core logic | **Rust** | The session state machine, event log and persistence live in Rust and are unit-tested |
| UI | React + TypeScript | The view layer only. The pixel-art theme is plain CSS (`image-rendering: pixelated`) |
| Storage | SQLite (local file) | Offline, serverless, a single file per machine |
| Releases | GitHub Actions (`tauri-action`) | Builds the Windows `.exe` / `.msi` and the Linux `.AppImage` / `.deb` on every tagged release |

---

## 🧩 Design Decisions

- **Events, not ticks.** Nudge does not count timer ticks, which drift and freeze when the computer sleeps. It writes each state change (`start`, `pause`, `interrupt`, `resume`, `end`) to SQLite with a timestamp the moment it happens, and computes every duration from those events. A power cut or a crash loses nothing, so there is no need for a periodic autosave.
- **Totals are derived, never stored.** A project's Total, Distracted and Real time are sums over its sessions, and Real is always Total − Distracted. Nothing can drift out of sync.
- **Single user per OS account.** Your operating system already separates users, so Nudge has no profiles or passwords.

## ⚠️ Known Limitations

- **Linux on Wayland (e.g. GNOME):** the compositor decides window stacking and may ignore "always on top" and global shortcuts. It works on Windows and on Linux with X11.
- Nudge only knows what you tell it. If you don't press Interrupt Work, it counts the time as focus. That is by design.

---

## 🗺️ Roadmap

- [ ] Event-based SQLite schema (`projects`, `sessions`, `session_events`, `distraction_types`)
- [ ] Session state machine in Rust, with unit tests
- [ ] Compact always-on-top window and global hotkey
- [ ] Projects list and session log with distraction breakdown
- [ ] Export to JSON
- [ ] CI release: Windows installer (`.exe`) on GitHub Releases
- [ ] Later: i18n (EN/ES), "Don't forget" reminders, pixel-art theme, import, tray icon

## 📄 License

[MIT](LICENSE)

---

*Built by [CrisLottz](https://github.com/CrisLottz) for people who are obsessed with their own productivity and want an honest number.*
