# BPMonitor

A single-file blood pressure log — one `index.html`, no build step, no frameworks, no external dependencies. Runs as a web app (GitHub Pages) and installs to an Android/iOS home screen as a PWA-style shortcut.

**Live app:** https://jovanbg.github.io/BPMonitor/

## Features

- Quick entry of systolic / diastolic pressure and pulse, with auto-advance between fields
- Left / Right arm switch; L+R readings taken within 10 minutes are visually grouped in the history
- Separate date and time fields, pre-filled with the current time
- Trend chart (systolic, diastolic, pulse) rendered as plain SVG — works offline, no CDN
- Averages across all readings
- History table with per-entry delete
- JSON export / import
- Dark theme

## Data storage & sync

Readings are always saved to the browser's `localStorage` first. Optionally, the app syncs to a **separate private GitHub repository** via the GitHub Contents API: every save commits a `data.json` file, so any device configured with the same repo and token sees the same data (and you get full history through git).

This repo contains **only the app**. No measurements are stored here.

### Setting up sync

1. Create a **private** repo (e.g. `bp-data`) with a README so it has a `main` branch.
2. Create a **fine-grained personal access token**: GitHub → Settings → Developer settings → Fine-grained tokens → access limited to that one repo → Repository permissions → **Contents: Read and write**.
3. In the app, open **⚙ GitHub sync settings**, enter owner, repo, `data.json`, and the token → **Save & connect**.

The token is stored only in the browser's `localStorage` on each device you configure. It is never committed anywhere and never leaves the device except in requests to `api.github.com`.

## Install on a phone

Open the live URL in Chrome → menu (⋮) → **Add to Home screen**.

## Data format

`data.json` is an array of readings:

```json
[
  {
    "id": "1758300000000-ab12c",
    "ts": 1758300000000,
    "sys": 120,
    "dia": 80,
    "pul": 68,
    "arm": "L"
  }
]
```

`ts` is a Unix timestamp in milliseconds; `arm` is `"L"` or `"R"` (may be absent on older entries).

## Disclaimer

Personal logging tool, not a medical device. Don't make treatment decisions based on it — talk to your doctor.
