# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"جسر | JISR" (formerly "مهارة مقابل قيمة | Skill for Value"): a platform matching university students/graduates with short paid tasks from organizations. `index.html` started as the original site from https://dashing-torte-81514a.netlify.app/ (only the Netlify-injected comment and `/.netlify/` script were removed) and has since been rebranded. It is published on GitHub Pages at https://reralalalmunis.github.io/jisr/ (repo `reralalalmunis/jisr`).

Brand assets live in `brand/`. `BRAND.md` has the name, tagline and colors. The images: `icon.png` (favicon and header mark), `logo-stacked.png` / `logo-stacked-dark.png` (home hero, light/dark), and `brand-board.png` (reference). The tagline is the `tagline` string in `index.html`.

## Structure

- `index.html` is the whole app: HTML, CSS (in `<style>`), and JS (in `<script>`) in one file. No build step, no dependencies, no package manager, no tests. Plain HTML/CSS/JS only.
- It is a single-page app with hash routes: `#/home`, `#/tasks`, `#/task/<id>`, `#/auth`, `#/business`, `#/profile`, `#/matching`, `#/review`, `#/projects`.
- Routing: `VIEWS` maps route names to view functions. `parse()` reads `location.hash` into `S.route`/`S.param`, and unknown routes fall back to `home`. `render()` draws the current view. `go('#/x')` navigates. A `hashchange` listener re-renders and scrolls to top.
- App state lives in the global `S` object (students, businesses, tasks, applications, UI state). Only the language and theme choices are persisted, via the `LS` wrapper around `localStorage` (keys `sfv-lang`, kept from the old name so saved choices survive, and `jisr-theme`). Everything else resets on reload.
- Bilingual: `S.lang` is `ar` or `en`. Strings go through `tr(key)`, and bilingual values go through `L(...)`. `applyLang()` sets `lang`/`dir` on `<html>`. Add both Arabic and English text for any new string.

## Running and checking

Open `index.html` in a browser. Add a hash to go to a route, e.g. `index.html#/tasks`. To screenshot it headlessly on Windows (run from the project folder):

```powershell
& "${env:ProgramFiles(x86)}\Microsoft\Edge\Application\msedge.exe" --headless=new --disable-gpu --hide-scrollbars --window-size=1280,3800 --screenshot="<out>.png" "file:///$($PWD -replace '\\','/')/index.html#/tasks"
```

Headless Edge won't render narrower than ~500px; a `--window-size=420,...` screenshot is cropped, not a real overflow. Use 500 for mobile checks.

## Conventions

- Arabic and RTL are the default (`<html lang="ar" dir="rtl">`).
- Colors come from the CSS variables in `:root`, with dark values in `:root[data-theme="dark"]`. Don't add raw hex values in new rules. The variable names are older than the rebrand: `--brand` is JISR blue, `--gold`/`--gold-fill` are the orange accent, `--ongold` is text on orange, and `--mint` is a pale blue tint.
- Dark mode does not follow the device setting. The site always starts light; the header sun/moon button (`themeBtn()`, action `A.theme`) toggles `S.theme`, which is saved in `localStorage` (key `jisr-theme`) and applied as `<html data-theme="dark">` by `applyLang()`. A tiny script in `<head>` sets the attribute before first paint to avoid a flash. Style dark mode only with `:root[data-theme="dark"]` selectors (the hero logo swap does this).
- Styles are desktop-first, with breakpoints at `max-width:980px` and `max-width:620px`.
- Use relative paths only. The site is served from `/jisr/` on GitHub Pages, so root paths like `/x` would break.
