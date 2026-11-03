# HANDOFF INSTRUCTIONS — for a Hermes Agent session on the Umbrel server

> You are Hermes. A user's earlier session on a different machine built an
> Umbrel **Community App Store** package for **Suwayomi** (a self-hosted manga
> reader server). The user is bringing this folder to you to continue. Read
> this file fully first, then carry out the task below.

---

## 1. Project context (what this is)

- **App:** Suwayomi — free, open-source manga reader *server* that runs
  Mihon/Tachiyomi-compatible extensions, readable from a browser. It serves a
  web UI on internal port **4567**.
- **Goal:** a working Umbrel app so the user can install Suwayomi from the
  Umbrel App Store on this server.
- **Delivery shape:** a "Community App Store" repo, because the app is not in
  Umbrel's official store.

## 2. Where everything is right now

The project folder is the directory containing this file. Its layout:

```
./                       # the repo root
├── HERMES_CONTINUE.md   # this file
├── TUTORIAL.md          # full beginner step-by-step guide (parts A–G)
├── README.md
├── umbrel-app-store.yml # store manifest (id: manga, name: Suwayomi App Store)
└── suwayomi/
    ├── umbrel-app.yml   # app manifest (media category, port 4567, version 0.1.0)
    ├── docker-compose.yml
    ├── icon.svg
    └── icon.png
```

The files are complete and valid. The container image is
`ghcr.io/suwayomi/suwayomi-server:stable` (verified real, multi-arch amd64+arm64).

## 3. Current status — NOT YET DONE

- ✅ App package built & YAML validated
- ✅ Tutorial written (`TUTORIAL.md`)
- ⬜ Published to a **public GitHub** repo
- ⬜ Store added to Umbrel UI
- ⬜ App installed & first-run configured
- ❓ **UNANSWERED:** the user has NOT confirmed whether an Umbrel server
  exists/works on this box yet.

**Hardware caveat the user was told:** Umbrel OS does NOT run on a Banana Pi
M2 Berry (Allwinner V40, 1 GB RAM). Umbrel needs a supported device (Raspberry
Pi 5 8 GB, or an x86_64 mini-PC). If you are running on something that is not
a supported Umbrel device, tell the user clearly rather than proceeding.

## 4. Your task (in order)

1. **Ask first (one question):** confirm whether this box is (a) running
   umbrelOS on supported hardware, or (b) something else. Do NOT assume —
   this changes the whole plan:
   - If (a): proceed to steps 2–5.
   - If (b): stop and instead offer to run Suwayomi's *same* Docker image
     (`ghcr.io/suwayomi/suwayomi-server:stable`, web UI port 4567, data at
     `/home/suwayomi/.local/share/Tachidesk`) directly with a plain
     `docker compose` file, or hand the user the `TUTORIAL.md` buying/setup
     guidance. Do not fabricate a store install on unsupported hardware.

2. **If umbrelOS confirmed:** verify this folder is clean for publishing —
   check `umbrel-app-store.yml` is at the repo ROOT, the app folder is named
   `suwayomi`, and `suwayomi/umbrel-app.yml` has NO leftover placeholders such
   as `[YOUR NAME]` / `[YOUR-USERNAME]` / `[YOUR-STORE-REPO]`. Replace any
   placeholders with the user's real values (ask for them).

3. **Publish to GitHub** (public repo). Two allowed routes; pick based on what
   tooling exists on this box:
   - `gh` CLI (`gh repo create <user>/umbrel-suwayomi-store --public --source . --remote origin --push`) — use if `gh` is installed and authenticated.
   - plain `git` + a repo created in GitHub web UI by the user.
   If neither is installed and you cannot install, tell the user exactly what
   to do in the GitHub web UI instead (see `TUTORIAL.md` Part C Method 1) and
   report the expected repo URL. The repo MUST be public and the store
   manifest at the root.

4. **Report the exact repo URL** the user should paste into Umbrel
   (App Store → Community App Stores → Add). Umbrel appends `.git`
   automatically, so a plain URL is fine.

5. **After the user adds the store:** help install the app and do first-run
   setup (Browse → Extensions → install sources). If the app install fails,
   check the app logs / `docker logs` — diagnose, don't guess.

## 5. How to know you're done

- [ ] User confirmed the hardware/OS situation.
- [ ] Store repo is public and pushable/added.
- [ ] Suwayomi shows in the Umbrel App Store under the community store.
- [ ] App is installed, launchable, and shows the Suwayomi web UI.

## 6. Rules

- Update `version` + `releaseNotes` in `suwayomi/umbrel-app.yml` whenever you
  bump the app.
- Preserve the exact files that already exist; only edit the placeholders.
- Never invent hardware facts or claim an install succeeded without verifying
  (app shows Running, web UI loads).
- Keep the user's beginner skill level in mind: step-by-step, plain,
  one clear path at a time.