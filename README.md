# Suwayomi — Umbrel Community App Store

Umbrel app package for **Suwayomi**, the self-hosted manga reader server
(compatible with Mihon / Tachiyomi extensions).

## Layout

```
Suwayomi-umbrel/
├── umbrel-app-store.yml      # community-store manifest (id: manga)
└── manga-suwayomi/           # the app package (folder name MUST match app id)
    ├── umbrel-app.yml        # app metadata shown in the umbrelOS UI
    ├── docker-compose.yml    # runs ghcr.io/suwayomi/suwayomi-server:stable
    ├── icon.svg              # downloaded from selfh.st/icons
    └── icon.png              # gallery asset
```

- Container image is multi-arch (amd64 + arm64), so it runs on x86 Umbrel
  boxes and Raspberry Pi / ARM installs.
- Port **4567** is the web UI (opened through Umbrel's auth proxy).
- Data is persisted to Umbrel's app data dir at
  `~/.umbrel/app-data/manga-suwayomi/data/` (library, downloads, settings, backups).

## Installing

1. Push this folder to your own public GitHub repo
   (`git init`, `git add .`, `git commit`, `git push`).
2. Copy your new repo URL — it must end in `.git` (a plain `.gitignore`
   isn't needed for an app-only store).
3. On your Umbrel, go to **App Store → Community App Stores → Add**, paste
   the repo URL, and it shows up.
4. Install **Suwayomi** from it.

> Published from https://github.com/LoloMojo/umbrel-suwayomi-store

## First-run setup

1. Open the app → **Browse → Extensions**, install the manga sources you want.
2. (Optional) Add Mihon/Tachiyomi on your phone pointing at your Umbrel —
   Suwayomi serves a Tachiyomi-compatible API automatically.

## Updating

When the image updates, bump `version` and fill in `releaseNotes` in
`manga-suwayomi/umbrel-app.yml`. For reproducibility you can pin a specific digest
instead of `stable`, but then you must update it each release.
