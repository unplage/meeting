# AGENTS.md

Client-side PWA for recording meetings (camera/mic → WebM) with background recording, PiP, and IndexedDB storage. Deployed to GitHub Pages at `https://unplage.github.io/meeting/` (subpath `/meeting/`).

## Repo shape

- No build system, package manager, tests, or linting. Do not add one.
- `index.html` — the entire app: inline CSS and one vanilla-JS script. No external deps, no modules, no imports.
- `sw.js` — service worker (cache-first for static assets, network-first for navigations).
- `manifest.json` — PWA manifest.
- `_headers` — Cloudflare Pages-format headers (COOP/COEP). Ignored by GitHub Pages.
- `README.md` and all UI copy/comments are in Chinese (zh-CN). Keep new user-facing strings and comments in Chinese.

## Conventions to preserve

- Code style is plain globals + inline `onclick="fn()"` handlers, single `<script>` block. Match this; don't refactor to a framework or module system.
- All asset paths are relative (e.g. `./sw.js`), never absolute, because the site is served from a subdirectory.
- `sw.js` derives `BASE_PATH` and its cache name from its own `location`; keep that dynamic logic intact so it works under `/meeting/` and any subpath.
- Recordings live only in IndexedDB (`MeetingRecorderDB` / store `recordings`) inside the browser — there is no server.

## Gotchas

- `sw.js` pre-caches `index.html` and serves static assets cache-first. After editing, the old service worker/cache may serve stale content — bump the `v1` suffix in `CACHE_NAME` or hard-reload / unregister in DevTools.
- Camera/mic APIs require a secure context (HTTPS or `localhost`); the SW also only registers there.
- Verify changes by serving statically (e.g. `python3 -m http.server`) and testing in a browser; there are no automated checks.
