# Documentation preparation record

## Current control

- Mode: autodoc/onepass, explicitly requested by the user on 2026-10-24.
- Scope: complete browser Tetris target from the user request; no later passes defined.
- Approval basis: first PROJECT approval remains active; ARCHITECTURE through TASKS checkpoints are preapproved by autodoc selection. No implementation range is selected by document approval.
- Stop: after DOCS-008, before DOCS-009 package QC, documentation integration or implementation.
- Tracking: enabled for preparation and implementation by existing root AGENTS.md. Git publication is authorized by the repository development request.
- Reporting: Standard after explicit bootstrap; Minimal and Explicit profiles remain available.

## Scope source

The user requests a classic playable baseline followed by hold, ghost, seven-bag, wall kicks, hard drop, selectable locking policies, conditional ghost visibility, penetrator bonuses and gameplay, six settings, and per-setting URL/cookie persistence. Publish a versioned GitHub release after each completed, verified, integrated implementation phase. The brief will retain the complete target and identify proposed interaction decisions before acceptance.

## Decisions and document evidence

| Task | Current evidence / decision basis |
| --- | --- |
| DOCS-001 | All four root guidance files retained byte for byte from initial commit 2bbb3c2d41fd91fa75d293d3b4fbd4d3750efeac. Initialization checks below passed. Checked task, this record and HANDOFF are submitted together. Publication and issue closure must be read back. |
| DOCS-002 | Proposed PROJECT SHA-256 `663c709b8e03488ca780f0dffd173a29395b5c908e8a151d71049728ad79d517`; human acceptance pending. Scope/content review against the user seed and root policies passed; all requested capability groups covered, proposed assumptions explicit. Boundary decisions below remain proposals. DOCS-002 stays unchecked. |
| DOCS-003–008 | Future document checkpoints preapproved by requested autodoc mode, subject to accepted PROJECT and required owner QC. |
| DOCS-009 | Not selected; remains pending at the autodoc stop. |

## Initialization

- Original root-file presence: all four present: AI_DISCLOSURE.md, SDD-MANAGER.md, AGENTS.md, SKILL_DISCOVERY_PROTOCOL.md. None copied or edited.
- Verification: exact byte comparison against the initial Git objects for all four retained files; root policies also remain unchanged. Local working tree and index were clean before owned setup.
- Continuation: all-present case permits continuation without an initialization approval pause.
- Discovery: no repository-local standalone skills or bundled plugins. Running package: installed SDD Manager 0.15.2; coordinator and selected owner entries loaded. Linux/Bash host, Git, Node 24.22.1 and Python 3.12.14 available. Windows-only shell constraints are inapplicable on this execution host. Browser installation is unnecessary for document-only preparation.
- Tracking: exact package milestone and all nine DOCS issues created and read back with unique identities and correct associations before any target write. Native milestone sdd-docs-main #1; issues #1 through #9 correspond to DOCS-001 through DOCS-009. All initially open. Fixed content came from the running package preview, not project-specific replacements.
- Manual provenance: default enabled. Seed e76fded2fc8c8a36ff87c0d4b05b714ddf6aaa6a from main base 2bbb3c2d41fd91fa75d293d3b4fbd4d3750efeac on design-docs/ai-logs. Explicit two-parent merge 58c2562326e16a4740a5848a9526b4d60bb7a42a on main. Both refs pushed and verified before design-docs/main setup. Both scaffold files match package bytes; no transcript recorded. Preserved package CRLF; CRLF-aware whitespace check passed.
- Credential exclusion: additive *.tkn rule preserves existing .obsidian and .trash rules; conventional token is ignored and untracked. No token values appear in records.
- Current reset: none.

## Initialization publication

Initialization commit ad20a3a was pushed and read back on design-docs/main before DOCS-001 issue #1 was closed and its completed state read back. The four original root files and browser/shell policies remain unchanged. The connected GitHub integration could read but returned 403 for comment/state writes; the supplied repository token API client completed closure and readback. Use that verified client for remaining authorized lifecycle writes.

## Proposed gameplay decisions for approval

These proposals resolve interactions absent from the initial description. Approval of this brief adopts them; they are not represented as requirements already supplied by the user.

| Concern | Proposed rule and rationale |
| --- | --- |
| Ghost at the midline | Use the topmost occupied ghost cell. Hide in dynamic mode if any ghost cell is above the midline; show only when the entire ghost is in the lower half. A straddling piece stays hidden. |
| Lock timing | Start a timer on becoming grounded, using the gravity interval at that moment. Grounded movement/rotation does not refresh it. Becoming airborne cancels it; a later grounding starts a full interval. Pause freezes timers. This prevents indefinite grounded stalling. |
| Failed-down policy | A failed gravity or soft-drop attempt locks a legal grounded piece. Hard drop with “follow general” counts its terminal failed descent as that attempt. |
| Delayed hard drop | Under the interval policy, hard drop starts the grounding delay if newly grounded, or preserves a running delay. Immediate mode locks at once. |
| Penetrator activation | Spend one bonus on successful activation, without refund; activation is one-way until lock and repeated activation has no effect. Hold is disabled while the penetrator is active, preventing storage or transfer of the ability. |
| Penetrator traversal and locking | Allow traversal through settled cells while respecting board boundaries. Ground only at the deepest valid landing at or below the current position; overlapping traversal cannot start a lock timer. If a piece reaches the board bottom without a legal resting position, end the game without overwriting settled cells. |
| Penetrator landing | Select the deepest nonoverlapping in-board position reachable by vertical traversal; intervening occupied cells do not stop descent. Never move the piece upward during hard drop. No valid landing yields no ghost and an ineffective hard drop; normal progression still enforces legal locking or game over. |
| Setting changes during play | Apply visibility and input toggles immediately. Changing a locking policy cancels the old timer and starts a full interval if the active piece is legally grounded under the interval policy; switching to failed-down waits for the next attempt. Changing hard-drop locking affects future hard drops only. |
| Invalid settings | Reject invalid URL/cookie values per setting and fall through to the next valid source. Invalid explicit URL values do not overwrite valid cookies. Use the first occurrence of repeated query parameters; preserve unrelated parameters and the URL fragment. |
| Defaults | Dynamic ghost, hard drop on, next preview on, hard drop follows general, full-interval general locking, hold on. |

## Pending transitions

Publish/read back the PROJECT proposal and present its exact state for human approval. Architecture and dependent authoring remain held. DOCS-002 issue #2 stays open; DOCS-003–009 have not started. Keep sdd-docs-main open. Subsequent publication is recoverable from Git readback; do not infer approval from a proposal commit.
