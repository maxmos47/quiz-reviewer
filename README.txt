EM Quiz Pool Reviewer (Realtime + WebApp sync + Activity Log) - Deploy bundle
=============================================================================

Files:
  index.html                    - the Reviewer (~73 KB, password-gated)
  pool.json                     - fallback pool for first load
  _redirects                    - Netlify routing helper
  firestore-rules.txt           - paste into Firebase Console -> Firestore -> Rules
  firebase-storage-rules.txt    - paste into Firebase Console -> Storage -> Rules

Features:
  - Password gate at boot (SHA-256 baked into HTML)
  - Real-time review sync via Firestore (reviews + edits + notes + images)
  - Cloud-synced pool: UPLOAD POOL pushes JSON to Storage and updates
    quizReviews/_meta so all reviewers auto-switch versions.
  - WebApp sync: each upload also batch-seeds the top-level quiz_pool
    Firestore collection so the EM High Yield Web App reads from the same
    source.
  - Host-added questions (h_*): created in Web app's mini reviewer and
    auto-imported into standalone Reviewer with a 'host quiz' pill.
  - Per-card Delete button: soft-deletes in quizReviews + hard-deletes from
    quiz_pool. Sync to Web app and other reviewers in real time.
  - Sort by question # in toolbar.
  - Smart numeric search: typing "1000" jumps to q_1000, "5" -> q_0005, etc.
  - Activity log: 📋 Activity drawer shows last 100 actions (who, what, when).

Firestore data model:
  /quiz_pool/{qid}                                  — Web App's question pool
  /quizReviews/_meta                                — pointer to the active pool
  /quizReviews/v{X.Y}/questions/{qid}               — per-question review state
  /quizReviews/v{X.Y}/audit_log/{auto_id}           — activity log entries

Firebase Storage paths:
  quizReviews/_pools/v{X.Y}.json                    — pool JSON
  quizReviews/v{X.Y}/{qid}.jpg                      — per-question images

Deploy on Netlify:
  - First time:  https://app.netlify.com/drop  ->  drag this folder.
  - Updates:     site dashboard -> Deploys tab -> drag this folder.
  - Reviewers:   Ctrl+Shift+R (Cmd+Shift+R on Mac) to hard-refresh.

Before sharing the URL the first time, paste both .txt rule files into
Firebase Console and click Publish for each.
