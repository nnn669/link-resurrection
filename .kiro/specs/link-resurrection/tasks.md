# Tasks — Link Resurrection

- [x] 1. Repo scaffold: README, MIT LICENSE, .gitignore that keeps `/.kiro`
      tracked, steering docs (product, tech).
- [x] 2. Spec: requirements, design, task list under
      `.kiro/specs/link-resurrection/`.
- [x] 3. Page shell: header, intake area (textarea, bookmarks import,
      demo button), stats bar, results list, export bar; dark Kiroween
      theme, mobile responsive.
- [x] 4. Intake logic: URL extraction/normalisation/de-dup, bookmarks
      HTML parsing, demo set.
- [x] 5. Checker: AllOrigins primary + CodeTabs fallback, verdict mapping
      (Alive / Dead / Unknown), concurrency pool of 3, 12s timeouts,
      progressive rendering.
- [x] 6. Resurrector: Wayback Availability API for Dead links, snapshot
      URL + date, Gone fallback.
- [x] 7. Export: standalone start-page HTML generation, copy + download.
- [ ] 8. Enable GitHub Pages on the repo; verify the hosted page on
      desktop and phone.
- [ ] 9. Final demo pass: run the demo set on the hosted page, confirm
      every verdict class renders correctly.
- [ ] 10. Devpost submission pack: Kiro usage write-up, screenshots,
      ≤3-minute demo video.
