# Requirements — Link Resurrection

## Introduction

A single-page tool that takes a list of old bookmark URLs, determines which
are still alive, revives dead ones from the Wayback Machine, and exports a
clean start page. Built for the Kiroween 2026 Resurrection track.

## Glossary

- **Link**: one URL supplied by the user.
- **Alive**: the link's origin responds with HTTP 2xx/3xx via a check proxy.
- **Dead**: the link's origin responds with HTTP 404 or 410 via a check proxy.
- **Unknown**: no check proxy could produce a trustworthy verdict.
- **Resurrected**: a Dead link for which the Wayback Machine holds a snapshot.
- **Gone**: a Dead link with no Wayback snapshot.

## Requirements

### Requirement 1: Link intake

**User story:** As a user, I want to paste many URLs at once or import my
browser's bookmarks export, so that I don't have to type links one by one.

#### Acceptance criteria

1. WHEN the user pastes text into the input area, THE SYSTEM SHALL extract
   every line that looks like a URL, adding `https://` when the scheme is
   missing, and de-duplicate the result.
2. WHEN the user selects a `bookmarks.html` file, THE SYSTEM SHALL parse all
   `href` values from it into the link list.
3. WHEN the user clicks "Load demo links", THE SYSTEM SHALL fill the list
   with a sample set covering Alive, Dead-with-archive and Gone cases.

### Requirement 2: Liveness checking

**User story:** As a user, I want each link labelled Alive, Dead or Unknown,
so that I know the true state of my bookmarks.

#### Acceptance criteria

1. WHEN checking starts, THE SYSTEM SHALL test links through a public CORS
   proxy that reports the target's HTTP status, with a second proxy as
   fallback, at most 3 checks in flight, each with a ~12 second timeout.
2. WHEN a proxy reports HTTP 2xx or 3xx, THE SYSTEM SHALL label the link
   Alive; WHEN it reports 404 or 410, THE SYSTEM SHALL label it Dead.
3. WHEN no proxy can verify a link, THE SYSTEM SHALL label it Unknown and
   SHALL NOT guess a verdict.

### Requirement 3: Resurrection

**User story:** As a user, I want dead links replaced by their archived
copies, so that the content I bookmarked is reachable again.

#### Acceptance criteria

1. WHEN a link is labelled Dead, THE SYSTEM SHALL query the Wayback
   Availability API for the closest snapshot.
2. WHEN a snapshot exists, THE SYSTEM SHALL mark the link Resurrected and
   use the snapshot URL as its effective address, showing the snapshot date.
3. WHEN no snapshot exists, THE SYSTEM SHALL mark the link Gone.

### Requirement 4: Results and export

**User story:** As a user, I want a summary and a clean page of my working
links, so that I can replace my old bookmark mess with it.

#### Acceptance criteria

1. WHILE results exist, THE SYSTEM SHALL show counts for total, Alive,
   Resurrected, Gone and Unknown.
2. WHEN the user clicks "Copy start page" or "Download start page", THE
   SYSTEM SHALL produce a standalone HTML page listing Alive links at their
   original address and Resurrected links at their archive address.
3. THE SYSTEM SHALL run entirely in the browser with no account and no
   backend, on desktop and mobile browsers.
