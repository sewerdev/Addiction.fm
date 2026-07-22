# [Addiction.fm](http://addiction.fm/)

**[Русская версия страницы](./Readme.ru.md)**

<img width="1920" height="943" alt="image" src="https://github.com/user-attachments/assets/113699d3-5d8a-4899-a9ce-a645774320bb" />

<img width="280" height="600" alt="image" src="https://github.com/user-attachments/assets/744b4274-d3a0-4d46-9767-5d77ae5a5b25" />


**Generative radio. One button - sound built for your mental state.**

Addiction.fm is a browser-based generative audio app with no playlists, no accounts, and no ads. Pick a state - get a unique sound that never repeats.

---

## Why this exists

Most music services require you to choose. Choosing is a distraction.
Addiction.fm removes the choice entirely: one button, one state, one stream of sound. Runs in the browser, no install required, no login needed.

---

## States

| State | Genre | For |
|-------|-------|-----|
| 🔴 **DRIVE** | Amen-break · Jungle · 180 BPM | Energy, task focus, movement |
| 🔵 **FOCUS** | Deep ambient | Sub-bass and dark pads only - no drums |
| 🟢 **CALM** | Slow drone + rain | Relaxation, sleep, recovery |

---

## How it works

Sound is **generated in real time** via the Web Audio API - this is not streaming, not a playlist. Every press creates a unique combination:

- **DRIVE** - amen-break sliced into 16th notes with DJ jumps and soft piano on top
- **FOCUS** - sub-bass synthesized in the chosen key, dark pads through a delay chain
- **CALM** - drone from detuned oscillators, filtered noise, slow bells

---

## Why Addiction.fm

- **Runs everywhere** - Chrome, Firefox, Safari, Android, iOS. Open and play.
- **Works offline** - download two files (`addiction-fm.html` + `breaks.js`) and run locally from any folder
- **No accounts** - no tracking, no ads, no recommendation algorithms
- **Always different** - the generative engine creates a new version on every press
- **Deterministic tracks** - the `‹‹ #N ››` counter locks a specific track by seed number so you can return to it
- **WAV recording** - the ⏺ rec button captures the track in real time and downloads it as `.wav`
- **Propeller** - spins during playback, gradually coasts to a stop on pause

---


Requirements: **modern browser** (Chrome 90+, Firefox 88+, Safari 15+). Nothing else.

---

## Tech stack

- **Web Audio API** - synthesis, slicing, mixing, real-time recording
- **OfflineAudioContext** - full track and stem rendering without artifacts
- **ScriptProcessorNode** - PCM capture for WAV export
- **Pure HTML/JS** - zero dependencies, zero frameworks, single file

---

## Author

Made by **Sewerbox** / [sewerdev](https://github.com/sewerdev)
Telegram: [@VestronVulture](https://t.me/VestronVulture)

---

*Addiction.fm is not a playlist. It's a state of mind.*
