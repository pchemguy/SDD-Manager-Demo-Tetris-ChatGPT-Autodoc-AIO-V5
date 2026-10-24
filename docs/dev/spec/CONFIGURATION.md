# Configuration contracts

## Settings schema

R09: settings have these canonical query names, encodings and defaults. Each cookie name is `tetris_` followed by its query name. Unknown fields are ignored. Values match exactly, case sensitively, after normal URL decoding; no whitespace trimming or numeric coercion is permitted.

| Setting / URL key | Valid values | Default |
| --- | --- | --- |
| Ghost visibility / `ghost` | `hidden`, `dynamic`, `shown` | `dynamic` |
| Hard drop / `hardDrop` | `0`, `1` | `1` |
| Next preview / `next` | `0`, `1` | `1` |
| Hard-drop lock / `hardDropLock` | `follow`, `immediate` | `follow` |
| General lock / `lock` | `failed`, `interval` | `interval` |
| Hold / `hold` | `0`, `1` | `1` |

Controls show effective typed values. Visibility/preview and hard-drop/hold toggles take effect immediately during any lifecycle state. Disabling preview does not consume or change queued pieces. Disabling hold preserves its slot and eligibility. General/hard-drop policy changes follow GAMEPLAY timing contracts. Settings changes do not resume paused play or restart a game. Penetrator state always overrides enabled hold.

## Restoration

R10: resolve each setting independently at startup using the first URL occurrence if valid, else the corresponding valid cookie, else the default. An absent or invalid value is not a valid source. Duplicate known cookie names select the first matching cookie occurrence; decoding failure rejects that cookie value only. One invalid setting never invalidates another setting's URL/cookie.

For each valid explicit URL value, write that exact canonical value to its cookie even if equal to the default or current cookie. Do not save defaults over cookies merely because a URL parameter is absent or invalid. Startup does not add missing query parameters or rewrite invalid ones. The full effective settings snapshot is established before creating the session.

Examples: `?hold=0` plus cookie `tetris_next=0` yields hold off and preview off with other fields independently restored/defaulted. `?hold=1` replaces a saved hold-off cookie. `?hold=invalid` falls back to a valid hold cookie without overwriting it. `?hold=0&hold=1` resolves to off; an invalid first occurrence followed by a valid second occurrence still falls through to cookie/default.

## Writes and failures

R11: after a user changes one field, retain its canonical typed value in memory, write only its cookie and update only its URL key using history replacement without navigation/reload or adding a history entry. Remove repeated occurrences of that key and retain one new value. Preserve unrelated query entries, path and hash; normal URL serialization may re-encode equivalent text. Unchanged settings must not acquire new cookies or URL parameters just because a different field changed.

Cookies use Path=/, Max-Age=31536000 (365 days), SameSite=Lax and Secure on HTTPS. No Domain attribute. Cookie name/value are encoded for safe cookie transport and decoded before validation. Cookies store settings only, never score, board or credentials.

Cookie and history writes are attempted independently. An exception or failed cookie readback leaves the effective in-memory setting selected and shows a nonblocking persistence warning naming the affected destination. A subsequent successful write clears its destination warning. If cookies are denied/unreadable, URL/default restoration remains available; if URL parsing fails unexpectedly, cookie/default resolution remains available. A failure must not overwrite other fields or prevent start/play. Reload guarantees apply when the corresponding browser storage/history operation succeeds; do not claim persistence after rejection.

Acceptance: exercise all valid values, missing/empty/unknown/case-invalid values, malformed encoding, duplicate URL/cookie keys and independent mixed-source restoration. Verify valid explicit defaults are saved, omitted URL fields preserve cookies, one UI change affects only its own cookie/query key, the fragment/unrelated content survives, and reload restores effective settings. Simulate denied cookie/history operations independently and verify selected in-memory values, warnings and playable sessions. Repeat representative restoration and change/reload flows on Chromium and Firefox.
