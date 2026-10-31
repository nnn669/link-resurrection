# Design — Link Resurrection

## Architecture

One static `index.html`; all logic in the page. Three stages:

```
intake ──► check (proxy pool) ──► resurrect (Wayback API) ──► render + export
```

## Components

### Intake
- Textarea parser: split lines, trim, prepend `https://` if schemeless,
  keep only strings containing a dot in the host part, de-duplicate.
- Bookmarks import: `DOMParser` on the exported HTML, collect
  `a[href^="http"]`.
- Demo set: curated URLs covering each verdict class.

### Checker
- Proxy chain per link:
  1. AllOrigins `https://api.allorigins.win/get?url=<enc>` — response JSON
     carries `status.http_code` from the target.
  2. CodeTabs `https://api.codetabs.com/v1/proxy?quest=<enc>` — plain
     fetch; a resolved 2xx response counts as Alive, errors as Unknown.
- Verdict mapping: 200–399 → Alive; 404/410 → Dead; everything else
  (401/403/5xx, network error, timeout, both proxies exhausted) → Unknown.
  Rationale: 403/5xx through a shared proxy often means bot-walling or
  proxy trouble, not a dead origin — honesty principle from steering.
- Worker pool of 3, `AbortController` timeout of 12s per attempt, results
  streamed into the UI as they land.

### Resurrector
- For Dead links only: GET
  `https://archive.org/wayback/available?url=<enc>`.
- `response.archived_snapshots.closest` present → Resurrected; effective
  URL = snapshot `url`, label includes the snapshot timestamp
  (formatted YYYY-MM-DD). Absent or request failed → Gone (a failed lookup
  leaves the link Dead/Gone, never fabricates a snapshot).

### Renderer / exporter
- Stats bar recomputed on every verdict change.
- Result rows: original URL, verdict chip, effective URL (original for
  Alive, snapshot for Resurrected), per-row open/copy actions.
- Export builds a minimal standalone HTML document (inline styles, two
  sections: "Alive" and "Resurrected from the archive") and offers
  clipboard copy + `.html` download.

## Error handling

- Proxy timeouts and 5xx from the proxy itself degrade to the next proxy,
  then to Unknown — never to a guessed verdict.
- Wayback API failure marks the link Gone only when the API answered with
  no snapshot; transport failure keeps the Dead label with a "couldn't
  check the archive" note in the row title.

## Testing

- Demo set exercises every verdict class end-to-end.
- Manual pass on desktop + phone browser against GitHub Pages hosting.
