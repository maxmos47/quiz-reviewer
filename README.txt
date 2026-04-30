EM Quiz Pool Reviewer (Realtime + Password Gate) - Deploy bundle
================================================================

Files:
  index.html                    - the Reviewer (~63 KB, password-gated)
  pool.json                     - fallback pool for first load
  _redirects                    - Netlify routing helper
  firestore-rules.txt           - paste into Firebase Console -> Firestore -> Rules
  firebase-storage-rules.txt    - paste into Firebase Console -> Storage -> Rules

Features in this build:
  - Password gate at boot (SHA-256 hash baked into index.html)
  - Real-time review sync via Firestore (reviews + edits + notes + images)
  - Cloud-synced pool: UPLOAD POOL pushes the JSON to Storage and writes a
    pointer at quizReviews/_meta so all reviewers auto-switch versions.
  - WebApp sync: each upload also batch-seeds the top-level `quiz_pool`
    Firestore collection so the EM High Yield Web App reads from the same
    source of truth.
  - Sort-by-question-# option in the toolbar.
  - Scroll-fixed footer + sticky editbox controls so the last question stays
    fully clickable.

Rotating the password later:
  1) Compute SHA-256 of the new password (any online SHA-256 tool, or
     `printf '%s' 'newpw' | sha256sum`).
  2) Open index.html, find `const PASSWORD_HASH = "..."` near the top of
     the IIFE, replace the hash, save.
  3) Re-deploy. Existing reviewers will be asked for the new password
     because their stored unlock token won't match.

Deploy on Netlify:
  - First time:  https://app.netlify.com/drop  ->  drag this folder.
  - Updates:     site dashboard -> Deploys tab -> drag this folder.
  - Reviewers:   Ctrl+Shift+R (Cmd+Shift+R on Mac) to hard-refresh.

Before sharing the URL the first time, paste both .txt rule files into
Firebase Console and click Publish for each. Without those, cloud sync
will say "permission-denied".
