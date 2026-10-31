# Product steering — Link Resurrection

## What this is

A single-page web tool for the Kiroween 2026 hackathon, Resurrection track.
Users paste old bookmark URLs (or import a bookmarks.html export); the tool
checks each link's liveness, revives dead links via the Wayback Machine, and
exports a clean start page.

## Product principles

1. **Honesty over magic.** A link we cannot verify is labelled "Unknown",
   never guessed as alive or dead. Judges and users must be able to trust
   every verdict.
2. **Zero friction.** No login, no backend, no build step. One HTML file,
   open it and it works — desktop and phone browsers alike.
3. **Resurrection is the story.** Every screen should make the
   dead → archived → reborn journey visible (stats: total / alive /
   resurrected / gone / unknown).
4. **Privacy by architecture.** Links are processed locally in the browser.
   The only outbound calls are the liveness proxies and the Wayback
   availability API needed to do the job.

## Audience

Hackathon judges first (30-second comprehension: what it does, why it fits
Resurrection, that it actually works), bookmark-hoarding users second.
