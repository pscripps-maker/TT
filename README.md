# Touchline Tracker — Complete website package

This ZIP contains the files needed to host Touchline Tracker as a static website / Progressive Web App (PWA):

- `index.html` — the app and its interface (includes the app JavaScript)
- `manifest.webmanifest` — PWA name, launch settings and icons
- `sw.js` — service worker for offline caching and updates
- `icon-192.png`, `icon-512.png`, `icon.svg` — app icons
- `app.js` and `inline-check.js` — supporting copies of the app script for reference/validation; the current `index.html` runs its embedded script

## Publish on GitHub Pages

1. Back up your Touchline Tracker data from inside the app before updating.
2. Extract this ZIP.
3. Upload **all the files in this ZIP** to the root of your existing GitHub Pages repository, keeping the filenames unchanged. `index.html` should be at the published root, not buried inside another folder.
4. Commit the changes and wait for GitHub Pages to publish.
5. Open the site using its HTTPS GitHub Pages URL, then refresh/reopen the app. HTTPS is required for service-worker/PWA installation; opening `index.html` directly as a local file will not enable offline PWA features.

## Visual formation controls

- Tap a player, then tap another player to swap positions.
- Tap a substitute and then the player to replace, or tap the player first and then the substitute.
- Save formation and Save starting lineup save the changes and close their popups.

## Data safety

The app stores team and match data in the browser's local storage on each device. Updating the website should not require clearing browser storage. **Do not clear site data/storage** as part of the update. Export a backup regularly, and especially before replacing the hosted files.


## Latest reliability fixes (r6)
- Corrected saved-match team/date/season/tournament filtering with metadata fallbacks.
- Visual formation redraws the existing selection after a swap/substitution.
- Penalty shootout records and redraws the next kick state immediately.
- Start-period buttons start the match clock when pressed before kick-off.
- Yellow feedback is triggered on click rather than pointer-down, so scrolling should not flash buttons.
- Live page order is players, match timeline, player time, then event totals.
- Team backup filenames include the selected team name and export date.


## Latest reliability fixes
- Added a live-page toggle to switch between single-button and player-card layouts.
- Visual formation swaps and substitutions now redraw the pitch in the open popup; the popup stays open until Save formation & close is pressed.
- Penalty shootout score, recorded kick list, turn and available takers update inside the existing popup after each kick.
- Yellow feedback is limited to actual clicks on match action buttons rather than pointer-down gestures that can begin scrolling.
- Updated the service-worker cache version so the revised files can be picked up after publishing.


## Team page saved-team selection update
- The Team page no longer requires a separate Load team button. Choose a saved team in the Saved teams dropdown to load its players, kit colours, shirt numbers and age-group setup into the editable fields below.
- If the current team has unsaved changes, the app asks whether to save or discard them before switching.
- Saved team records now retain the age group as well.


### Popup refresh reliability fix (v10)
- Visual formation now updates the pitch inside the existing popup without rerendering the whole app while selections are being made. Changes remain in the popup until Save formation & close.
- Penalty shootout has a stable modal content container; each kick updates score, recorded kicks, turn and available takers in place without rebuilding the underlying live screen.
- Bumped service-worker cache to ensure updated files can be fetched.


Reliability update: undo snapshots are now capped by both count and total size. Saving first trims undo history and, if browser storage is full, retries after clearing the current match undo history. Visual formation and shootout saves use the same persistence routine. Older undo steps may be discarded to protect the match data.


## Build and diagnostics (2026-10-15)

- Release build ID: `TT-2026.10.15-r1`
- Service-worker cache: `touchline-tracker-20261015-r1`
- The app opens diagnostics after five taps on the top/bottom match banner within about 1.8 seconds. It reports build identifiers, active service worker, cache names, IndexedDB availability, and record counts.
- IndexedDB database: `touchline-tracker-db` (version 1), object stores `app` and `meta`. Existing localStorage data is retained and mirrored to IndexedDB; if localStorage is absent, the app attempts to restore the IndexedDB snapshot. This release deliberately keeps localStorage compatibility while IndexedDB rollout is tested.
- Statistics fixes normalise saved match records and ensure a selected team does not hide the saved-match statistics merely because the *current unsaved match* belongs to another team.

### Verify the running build
1. Publish every file in the ZIP to the repository root and commit.
2. Open the GitHub Pages site, tap the top/bottom match banner five times quickly, and compare the reported build ID and cache name above.
3. Check that the active service worker is present and IndexedDB says Available.
4. Create two teams and save one match with a goal and assist for the first team. Select that team in Statistics and History; its saved match and player event counts should appear. Select the second team; the first team's match should be excluded.

**Important technical limitation:** this is a transitional IndexedDB-backed release: localStorage remains the synchronous compatibility store because the existing application reads its state synchronously at startup. Data is mirrored into IndexedDB and can be restored from it when localStorage is absent, but this is not yet a complete localStorage-free IndexedDB-only rewrite.


## Release TT-2026.10.16-r6

- Penalty shootout first-kicker choice is now inside the shootout setup popup.
- Match substitutions update formation slots so the visual formation follows live substitutions.
- Team data import accepts both selective team backup files and full Touchline Tracker JSON backups.
- Statistics player table now aggregates filtered saved/current match records and honours team, season/tournament, and date filters.
- Check `version.json` and open Settings and tap the small version link at the very bottom to open diagnostics.

File version IDs: `index.html`, `app.js`, `inline-check.js`, and `manifest.webmanifest`: `TT-2026.10.16-r6`; `sw.js` cache: `touchline-tracker-20261015-r6`. `version.json` contains SHA-256 hashes for the shipped files.


## Release TT-2026.10.16-r6

- Replaced the five-tap top-banner gesture with a discreet “version TT-2026.10.16-r6” text link at the bottom of Settings. Tapping it opens build diagnostics.
- Updated the service-worker cache identifier so the new build can replace the cached release.


Release r4 fixes the Settings version diagnostics link, wires the Team page import button to the device file picker, and rebuilds History / Stats filters to use team, season and competition type only (no date filters or filter opt-out checkbox).


## r6 fixes

- The version link opens diagnostics immediately and then fills in browser storage/cache details.
- Penalty shootout first-team selection has a direct Start shootout action.
- Statistics and History use the same team, season and competition filters; All includes every saved match.
- Shot/free-kick/penalty outcome buttons record the chosen outcome and close the popup.
- Save formation and Save starting lineup close the modal before rerendering.


## r6 reliability fixes
- Version link label is generated from the active build ID and opens diagnostics through one delegated click handler.
- Startup reconciles localStorage with the IndexedDB mirror and restores the mirror when it contains more saved match records than localStorage.
- History and statistics share a normalized saved-match reader; history explicitly reports the number shown versus stored and filters only by the selected team, season, and competition.


## r6 reliability fixes
- Version label is generated from the active build ID and opens diagnostics.
- Startup reconciles localStorage with the IndexedDB mirror and restores the mirror when it contains more saved match records than localStorage.
- History and statistics share normalized saved-match data; history reports matches shown versus stored and filters only by the selected team, season, and competition.


## Release TT-2026.10.16-r9

- The Settings version link is bound as soon as the script starts, so a later startup error cannot leave it inert.
- The service worker uses network-first for page navigation, reducing the chance that GitHub Pages keeps displaying an older embedded script after publishing.
- The Saved matches panel now separately shows an active, eventful current match when it has not yet been saved to history, with a direct “Save current match to history” action. It is clearly labelled as current and is not counted as a saved record until the action is used.
- Storage writes include a timestamp; startup can prefer a newer IndexedDB mirror when the saved-match counts are equal, while still preferring a copy with more saved matches.
- No IndexedDB file is needed in the ZIP: the browser creates the database.


## r9 data integrity fixes
- Factory reset clears the app's localStorage entries and IndexedDB object-store records before reloading, preventing old match data from being restored after reset.
- Saved-match deletion no longer re-archives the same current match when that record is deleted.
- Statistics filter options are refreshed before calculating player-stat totals to avoid stale selections.
- Player statistics heading clarifies that totals apply to filtered matches.
- Service worker cache version incremented and app code resources use network-first with offline fallback.


## r9 saved-match reliability fix

- The live Save match action and the Saved matches page button call the same explicit save routine.
- Explicit saving supports 0–0 matches as long as a team or opponent has been entered.
- Starting a new match archives the previous meaningful match before creating the new one.
- Saved matches are upserted by a stable history key to avoid duplicate records.
- The current-match prompt is wired directly after rendering, so it does not depend solely on document-level delegated click handling.
