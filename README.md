# UI Interviews

A lightweight, static coding environment used to evaluate UI/UX designer candidates on **Bootstrap 5** and **jQuery**. Each exercise is a plain HTML page that the candidate edits directly — no framework, no bundler, no build step for the markup.

## Requirements

- [Node.js](https://nodejs.org/) 18 or newer (includes npm)
- A modern browser
- A code editor (VS Code recommended)

## Installation

```bash
git clone <repository-url>
cd ui-interviews
npm install
```

`npm install` pulls Bootstrap, Popper, jQuery, Dart Sass, and Browsersync into `node_modules`. Run `npm run css` (or `npm start`) once after installing to generate `assets/css/styles.css`.

If npm prompts about an unapproved install script for `@parcel/watcher`, it is a Browsersync dependency and can be approved or ignored; the server works either way.

## Running the environment

```bash
npm start
```

Then open <http://localhost:3000>. The index page lists every exercise.

## Scripts

| Script              | Purpose                                                                                                                 |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `npm start`         | The normal way to work. Compiles CSS once, then runs the Sass watcher and the dev server together.                      |
| `npm run serve`     | Dev server only (Browsersync) on port 3000, with a control UI on port 3001. Live-reloads when HTML, CSS, or JS changes. |
| `npm run css`       | One-off compile of `assets/scss/styles.scss` into `assets/css/styles.css` with a source map.                            |
| `npm run css:watch` | Same compile, re-run automatically whenever a `.scss` file changes.                                                     |
| `npm run build`     | Alias for `npm run css`; the production-style build for this project.                                                   |
| `npm test`          | Placeholder — there is no test suite.                                                                                   |

Stop any running script with `Ctrl + C`.

## Project structure

```
index.html              Landing page linking to each exercise
exercise-01..06.html    Exercise pages (same asset references as the index)
assets/
  css/
    styles.css          Compiled output — do not edit by hand
  js/
    vendor/             jquery.min.js, popper.min.js, bootstrap.min.js
  scss/
    styles.scss         Entry point; imports Bootstrap's source then _custom.scss
    _variables.scss     Local copy of Bootstrap's variables — edit to theme
    _variables-dark.scss  Local copy of Bootstrap's dark-mode variables
    _custom.scss        Project styles and overrides
```

## How the pages are wired

Every page loads the same assets in the same order:

```html
<link rel="stylesheet" href="assets/css/styles.css" />
...
<script src="assets/js/vendor/jquery.min.js"></script>
<script src="assets/js/vendor/popper.min.js"></script>
<script src="assets/js/vendor/bootstrap.min.js"></script>
```

Order matters: jQuery first, Popper before Bootstrap (Bootstrap's dropdowns, tooltips, and popovers depend on it).

`assets/css/styles.css` is the full Bootstrap framework compiled from source plus the project's own styles, so no separate Bootstrap stylesheet link is needed.

## Styling workflow

- Edit **`assets/scss/_custom.scss`** for new rules and component overrides.
- Edit **`assets/scss/_variables.scss`** to change Bootstrap defaults (colors, spacing, fonts) before the framework compiles.
- Never edit `assets/css/styles.css` — it is regenerated on every build.
- Keep `npm start` running so changes compile and reload automatically.

## Notes for candidates

- Work only inside the marked candidate area of the exercise page unless told otherwise.
- Bootstrap 5 and jQuery are already loaded — no CDN links or installs are required.
- Reference docs: [Bootstrap 5](https://getbootstrap.com/docs/5.3/) and [jQuery API](https://api.jquery.com/).
- The browser refreshes automatically when a file is saved; a hard refresh (`Ctrl + Shift + R`) clears stale CSS if something looks wrong.

## Troubleshooting

- **Port 3000 already in use** — stop the other process, or run `npx browser-sync start --server --port 3001`.
- **CSS changes not appearing** — confirm `npm run css:watch` (or `npm start`) is running and check the terminal for Sass errors.
- **`sass` or `browser-sync` not found** — run `npm install`.
- **Bootstrap components not interactive** — check the browser console; usually a script tag is missing or out of order.
