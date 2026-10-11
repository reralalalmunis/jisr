# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Static rebuild of the "مهارة مقابل قيمة | Skill for Value" landing page — a platform matching university students/graduates with short paid tasks from organizations. Original site: https://dashing-torte-81514a.netlify.app/

`reference/REFERENCE.md` is the spec: section order, exact Arabic copy, colors, and layout rules. The JPGs in `reference/` are mobile-width screenshots of the original. Check changes against both.

## Structure

- `index.html` is the whole site: HTML, CSS (in `<style>`), and JS (in `<script>`) in one file. No build step, no dependencies, no package manager, no tests.
- Plain HTML/CSS/JS only. Login, sign-up, payments, and the English button are placeholder `href="#"` links.

## Running and checking

Open `index.html` in a browser. To screenshot it headlessly on Windows (run from the project folder):

```powershell
& "${env:ProgramFiles(x86)}\Microsoft\Edge\Application\msedge.exe" --headless=new --disable-gpu --hide-scrollbars --window-size=1280,3800 --screenshot="<out>.png" "$PWD\index.html"
```

Headless Edge won't render narrower than ~500px; a `--window-size=420,...` screenshot is cropped, not a real overflow. Use 500 for mobile checks.

## Conventions

- `dir="rtl"`, `lang="ar"`. Arabic is the primary language; Latin strings (e.g. "Skill for Value", task IDs) get `direction:ltr`.
- Colors come only from the CSS variables in `:root` (copied from REFERENCE.md). Don't add raw hex values.
- Radii: `--r-btn` (8px) for buttons, `--r-card` (14px) for cards. Max content width 1180px (`.wrap`).
- One breakpoint: `@media (min-width:860px)` switches to the desktop layout (two-column hero, 3-column grids, 5-column steps). Mobile styles are the default.
- The "how it works" toggle uses ARIA tabs (`role="tab"`/`tabpanel`) and the `hidden` attribute. A global `[hidden]{display:none !important}` rule is required because `.steps` sets `display:grid`.
- Steps for organizations and the FAQ answers aren't in the reference; they were written for this build and can be changed.
