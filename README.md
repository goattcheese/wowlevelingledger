# Forever Duo Ledger, web version

`index.html` is the published Ledger with its artifact storage swapped for Firebase (Firestore + email sign-in). Nothing else in the page changed. The "Ask Claude" button is hidden here because it only works inside Claude.

## One-time setup (about 15 minutes, on your computer)

1. Go to console.firebase.google.com, create a project (free Spark plan, no card). Skip Google Analytics.
2. Build > Firestore Database > Create database. Pick a region close to you, start in production mode.
3. Build > Authentication > Get started > Sign-in method > Email/Password > enable, and Google > enable.
4. Authentication > Users > Add user, twice: one for you, one for your cousin (any email and a password you pick; he never needs a Google or Claude account).
5. Firestore > Rules: paste `firestore.rules` with both emails filled in, then Publish.
6. Project settings > Your apps > Web (</>) > register an app. Copy the config values into `firebase-config.js`.
7. Put the three files (index.html, firebase-config.js, and nothing secret) in your GitHub repo and turn on GitHub Pages (Settings > Pages > deploy from branch). Send your cousin the page link plus his email and password.

## Notes
- Data lives in the same collections as before (slots, pairs, runs, cq, qp, wl, sessions, mail, live, patch, patchok, tal, legacy, sched, launch, backups).
- Use the Backup tab in the old artifact to save your data, then restore it here. The Backup tab does not cover legacy, tal, launch or sched, so those need a separate copy (planned).
- The weekly patch-check task currently writes to the artifact's database. It will need pointing at Firestore after the move.
- Sync from game works as before (file picker). Send to game downloads LedgerData.lua as before.
- Characters (since Oct 27, 2026): each `slots/<group>-<c|s>` doc is a seat. `made: true` means the seat holds a character; `tag` is `main` or `alt` and `style` is `duo`, `flex` or `solo`. Two made seats in the same group form a pair. Slot docs saved before this change have no `made` field and count as characters. Groups A to R come from the original plan; new groups (S, T, ...) are added when every group is in use.
