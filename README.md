# Link Resurrection

**Paste in your old bookmarks. We check which ones still live — and bring the dead ones back from the archive.**

Link Resurrection is a single-page, zero-backend tool built for the
[Kiroween 2026](https://kiroween.devpost.com/) hackathon, **Resurrection** track.

Old bookmark folders are graveyards: half the links are dead. This tool walks
the graveyard for you:

1. **Check** — every link is tested and labelled *Alive*, *Dead*, or *Unknown*
   (when a site refuses to be verified, we say so instead of guessing).
2. **Resurrect** — every dead link is looked up in the
   [Wayback Machine](https://web.archive.org/); if an archived copy exists, the
   link is revived and points at the closest snapshot.
3. **Reborn start page** — one click exports a clean new start page: live links
   keep their original address, resurrected links point at their archived copy.

## Try it

Open `index.html` in any modern browser — no install, no account, no server.
The hosted version runs on GitHub Pages (link in the repo About section).

You can:

- paste URLs (one per line), or
- import a browser bookmarks export (`bookmarks.html`), or
- hit **Load demo links** to see a live / dead / archived / gone mix instantly.

## How it works

Browsers block direct liveness checks to arbitrary sites (CORS), so checks are
routed through free public CORS proxies (AllOrigins, with CodeTabs as a
fallback). A link is *Alive* on an HTTP 2xx/3xx response through the proxy,
*Dead* on 404/410, and honestly *Unknown* when no proxy can verify it — the
tool never fabricates a verdict. Resurrection uses the free Wayback
Availability API (`archive.org/wayback/available`). Everything runs locally in
your browser; no link you paste is stored or sent anywhere except the proxy /
archive lookups themselves.

## Built with Kiro

This project was spec-driven with [Kiro](https://kiro.dev/). The `/.kiro`
directory in this repo is tracked on purpose:

- `.kiro/specs/link-resurrection/` — requirements, design and task list the
  implementation follows.
- `.kiro/steering/` — product and tech steering docs used across sessions.

## License

MIT — see [LICENSE](LICENSE).
