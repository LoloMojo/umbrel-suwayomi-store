# Suwayomi on Umbrel — Complete Step-by-Step Project Guide

Everything you need to go from zero to reading manga in your browser, served
by an Umbrel home server. Written for someone who hasn't done this before.

---

## Part A — The big picture (read this first)

We made an **"app package"** for Umbrel. An Umbrel app is just a folder with
two small files. But to install an app that isn't in Umbrel's official store,
you put that folder on **GitHub**, point your Umbrel at it as a *Community App
Store*, then press Install in the Umbrel UI.

```
[Your files]  ->  [GitHub repo (the "store")]  ->  [Umbrel App Store UI]  ->  [Running Suwayomi]
```

What each piece does:

| Piece | What it is |
|---|---|
| `suwayomi/` folder | The app package (manifest + docker-compose + icon) |
| `umbrel-app-store.yml` | Says "this GitHub repo is a store" (our store id: `manga`) |
| GitHub | Free place to host the repo; Umbrel reads it from there |
| umbrelOS | The server OS; it reads the store, downloads the app's Docker image, and runs it |

The actual program that runs is **Suwayomi server** — it pulls the official
Docker image `ghcr.io/suwayomi/suwayomi-server:stable` and serves a web app on
port 4567 inside Umbrel.

---

## Part B — Prerequisites ⚠️ IMPORTANT

**You need a machine actually running umbrelOS.** This is the one thing we
can't do for you, and it's a real hardware question:

- ✅ **Supported:** x86_64 PC/mini-PC/NUC, or a Raspberry Pi 5 (8 GB ideal).
- ❌ **Not supported:** your **Banana Pi M2 Berry** (Allwinner V40, 1 GB RAM).
  Umbrel OS doesn't build for that board, and 1 GB RAM is far too little —
  Umbrel expects 8 GB+ and dozens of GB of storage for apps plus Docker.
  Don't try to force it; it won't install.

If you don't have an Umbrel box yet, the two realistic options are:

1. **Buy one** (recommended): a Raspberry Pi 5 with 8 GB and a good microSD or
   NVMe, or any used/cheap x86 mini-PC (Intel/AMD N100-class is plenty). Install:
   - **Raspberry Pi:** download the umbrelOS image from umbrel.com, write it
     with **Raspberry Pi Imager** or Balena Etcher, insert the SD card, boot,
     then follow the on-screen setup in a browser at `http://umbrel.local`.
   - **x86 PC:** download the x86 ISO, and install it to the machine (it needs
     to be able to boot from USB).
   
   (The YouTube video you shared — MSI on Windows — is a *different* Suwayomi
   desktop install. For Umbrel you do NOT use that; the Docker route replaces it.)

2. **Skip Umbrel for now** and still run Suwayomi: you can run the exact same
   Docker image on almost any Linux box you already have. Tell me what you
   want and I can give you those steps instead.

> ⏭️ **If you already have Umbrel running, skip to Part C.**

---

## Part C — Publish the store to GitHub

Two ways. **Method 1 (no installs, all in the browser)** is what I'd start with.

### Method 1 — Web browser only (easiest)

1. Go to **github.com** and sign in (or create a free account).
2. Click **+** (top-right) → **New repository**.
3. **Repository name:** type `umbrel-suwayomi-store`
4. **Public** must be selected (Umbrel can't read a private store repo).
5. Leave "Add a README" etc. **unchecked** (we're uploading our own files).
6. Click **Create repository**.
7. On the empty repo page, click **"uploading an existing file"** (or the
   **Add file ▾ → Upload files** button).
8. **Drag the whole `Suwayomi-umbrel` folder** into the browser window. Keep
   the folder structure — it should contain:
   ```
   umbrel-app-store.yml
   README.md
   suwayomi/
     umbrel-app.yml
     docker-compose.yml
     icon.svg
     icon.png
   ```
   (Dragging a folder preserves the sub-folder. If you only see the files,
   re-create the `suwayomi/` folder and put those four files inside it.)
9. Click **Commit changes**.
10. Your repo URL is now:
    `https://github.com/YOUR-USERNAME/umbrel-suwayomi-store` — copy it.

> **Before publishing**, open `suwayomi/umbrel-app.yml` and replace the
> placeholders `[YOUR NAME]`, `[YOUR-USERNAME]`, `[YOUR-STORE-REPO]` with your
> real values. You can edit files right on GitHub (click the file → pencil
> icon → edit → Commit changes).

### Method 2 — Git CLI (better for future updates)

Useful once you'll update the app regularly. From a terminal (on Windows,
use Git Bash which comes with the Git installer):

1. Install Git: download from **git-scm.com**, install with defaults.
2. Create the empty **Public** repo on GitHub as in Method 1 (steps 2–6).
3. Open Git Bash and run (change the username/repo lines to yours):

   ```bash
   cd "D:/Hermes/Suwayomi-umbrel"

   # Set your identity once (only needed the first time)
   git config --global user.name  "Your Name"
   git config --global user.email "you@example.com"

   git init
   git add .
   git commit -m "Add Suwayomi Umbrel community app store"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/umbrel-suwayomi-store.git
   git push -u origin main
   ```

4. Refresh the GitHub page — files should be there.

**Useful: edit the folder before your change, then push again:**
```bash
cd "D:/Hermes/Suwayomi-umbrel"
git add .
git commit -m "bump version"
git push
```

---

## Part D — Add the store to Umbrel & install the app

On your Umbrel:

1. Open the Umbrel dashboard in a browser (`http://umbrel.local` or its IP).
2. Go to **App Store**.
3. Scroll down to **Community App Stores** → **Add**.
4. Paste the repo URL:
   `https://github.com/YOUR-USERNAME/umbrel-suwayomi-store`
   (You don't need to add `.git` — Umbrel appends it automatically.)
5. A new store box appears. Open it and you'll see **Suwayomi**.
6. Click **Install**. Umbrel pulls the official Docker image — this can take a
   minute or two on first install.
7. When done, click **Launch** / **Open**.

---

## Part E — First run of Suwayomi (5 minutes)

1. You land on the Suwayomi web UI.
2. On the left, go to **Browse → Extensions**.
3. Click **Settings** (near Extensions), and under *Extension sources* make
   sure the default source (Keiyoushi / Tachiyomi) is enabled. If the list is
   empty, click **Add**, paste the default repo
   `https://raw.githubusercontent.com/keiyoushi/extensions/repo.index` — or
   whichever source you set in the YouTube tutorial — and confirm.
4. Back on **Extensions**, install the manga sources you want (e.g. MangaDex).
5. Go to **Browse** → pick a source → search/add manga → read.

Optional: in the **Mihon/Tachiyomi** mobile app you can point to your Umbrel
server (settings → browse → sources) to read on your phone too.

---

## Part F — Update the app when a new Suwayomi version arrives

1. Open `suwayomi/umbrel-app.yml` (edit on GitHub, or locally then re-push).
2. Bump `version` (e.g. `0.1.0` → `0.1.1`) and write a short note in
   `releaseNotes`.
3. Commit + push.
4. In Umbrel, the app shows an **Update** button. Click it.

> For *reproducibility* you can pin the exact image digest instead of
> `stable` in `docker-compose.yml`, but then you must update it on every
> release. `stable` is fine for personal use.

---

## Part G — Troubleshooting

| Symptom | Fix |
|---|---|
| Store doesn't appear after adding | Wait ~30s, or check the repo went to **Public**, and that `umbrel-app-store.yml` sits at the repo **root** (not inside a folder). |
| App install fails / stuck | Check logs: Umbrel settings → app → logs, or SSH into the box and run `docker logs` on the suwayomi container. Usually a network/blocks. |
| Can't reach the web UI | Confirm the app says **Running**; open via Umbrel's Launch button (not a direct IP) so the auth proxy is used. |
| "Image not found" | Your network can't reach `ghcr.io`. Nothing wrong with the app. |

**Safety rule for the project folder:** keep a copy of `Suwayomi-umbrel/`
somewhere (e.g. Git or a USB stick). Recreating it is easy, but not losing it
is easier.

---

## Checklist before you publish to GitHub

- [ ] `umbrel-app-store.yml` at repo root (id: `manga`)
- [ ] `suwayomi/` folder with `umbrel-app.yml` + `docker-compose.yml` + icons
- [ ] placeholders in `umbrel-app.yml` replaced with your real name/repo
- [ ] repo is **Public**
- [ ] you have an actual Umbrel device (Raspberry Pi 5 / x86) to run it on
