# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static website (plain HTML + one CSS file, no build system, no JS, no tests) listing AAATechnologies' Android apps. Remote: `github.com/aaatechnology/applications`. The root `index.html` is the app listing; each app has a directory `<slug>/index.html` with its details page.

## Generator: `gen.py`

Almost every page here is **generated** by `gen.py`, not hand-written:

- `index.html` (listing), every `<slug>/index.html` (details page), and `assets/site.css` are all overwritten in full on each run. Edit `gen.py` (the `HEAD`/`FOOT` templates, the `apps` list, the inline CSS string) rather than the output files, or the change will be lost on the next run.
- `assets/icons/<slug>.png` are downloaded from the Play Store `og:image` (resized to `=s192`).
- App descriptions/taglines are scraped from saved Play Store pages at `play/<package>.html`. That `play/` directory is **not in this repo**.
- Paths are relative to the **parent** directory: `OUT = "applications"` and it reads `play/...`, so run it from one level up:
  ```sh
  cd .. && python3 gen.py   # or: python3 applications/gen.py, with play/ present in the parent
  ```
  It needs network access (icon downloads).

Adding a Play Store app = add a `(package, slug, display name)` tuple to `apps` and provide `play/<package>.html`. Astrosoft Tamil (`tamil-astrology`, package `com.aatech.astrology`) is special-cased via `entries.insert(0, ...)`: hardcoded description, no Play link (`soon=True` → "Coming soon" badge and a "ஆ" placeholder icon instead of a PNG), and `legal=True` → links to its legal pages.

## Hand-written pages

`tamil-astrology/legal/` (`index.html`, `terms.html`, `privacy.html`, `style.css`) is **not** produced by `gen.py` and has its own stylesheet, separate from `assets/site.css`. Edit those files directly.

## Conventions

- Details pages link assets with `../` (root-relative depth is baked into the `root` template variable); listing uses no prefix.
- Shared footer: `© 2026 AAATechnologies · aaatech2014@gmail.com`.
- CSS uses `:root` color variables with a `prefers-color-scheme: dark` override; the font stack includes `Noto Sans Tamil` for Tamil text.

## Previewing

No build step — open `index.html` directly or serve the directory, e.g. `python3 -m http.server` from the repo root.
