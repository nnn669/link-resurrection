# Tech steering — Link Resurrection

## Stack

- One static `index.html` (inline CSS + JS). No framework, no bundler,
  no dependencies. Hosted on GitHub Pages.
- Liveness: public CORS proxies — AllOrigins `/get` first (returns the
  target's HTTP status code), CodeTabs proxy as fallback.
- Resurrection: Wayback Availability API
  `https://archive.org/wayback/available?url=<url>`.

## Constraints that shape the design

- Browser CORS forbids direct fetches to arbitrary origins, and `no-cors`
  fetches cannot reveal status codes — a proxy that reports the target
  status is the only honest way to judge liveness client-side.
- Verdict mapping: HTTP 2xx/3xx → Alive; 404/410 → Dead; anything else,
  proxy failure, or timeout → Unknown (then still try the archive lookup
  only for Dead links).
- Checks run with small concurrency (3 in flight) so long bookmark lists
  don't hammer the free proxies; each attempt has a ~12s timeout.
- All state lives in the page. Export generates a standalone HTML start
  page string client-side (copy to clipboard or download).
