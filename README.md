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


## Latest reliability fixes (v7)
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
