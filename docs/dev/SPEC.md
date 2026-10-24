# System specification

## Context and authority

The [project brief](PROJECT.md) defines the complete target. [ARCHITECTURE](ARCHITECTURE.md) and [DECOMPOSITION](DECOMPOSITION.md) define structural ownership. This specification defines the intended complete behavior; PLAN assigns coherent subsets to releases. The application is a static, single-player, keyboard-first desktop browser game, with no account, server, multiplayer or saved game.

## Contracts and owners

| ID | Required guarantee | Component owner / detailed contract |
| --- | --- | --- |
| R01 | Legal tetromino spawn, movement, rotation, falling, locking and row clearing; no invalid board writes | C1/C2, [gameplay](spec/GAMEPLAY.md#board-and-pieces) |
| R02 | Predictable lifecycle, pause/restart/focus handling and keyboard commands | C1/C6, [lifecycle and input](spec/GAMEPLAY.md#lifecycle-and-input) |
| R03 | Score, lines, level and gravity progression update on locks and clears | C1/C5, [progression](spec/GAMEPLAY.md#progression) |
| R04 | Full gravity-interval default locking and selectable failed-down policy; hard drop follows or immediately locks | C1/C5, [timing](spec/GAMEPLAY.md#gravity-and-locking) |
| R05 | Seven-bag generation, one next preview and once-per-lock hold | C1/C3/C8, [sequence and hold](spec/GAMEPLAY.md#sequence-and-hold) |
| R06 | Shared hard-drop/ghost landing, with hidden/dynamic/shown visibility | C4/C8, [landing and ghost](spec/GAMEPLAY.md#landing-and-ghost) |
| R07 | Consumable penetrator, legal lowest landing, no settled-cell destruction and disabled active-penetrator hold | C1/C2/C4, [penetrator](spec/GAMEPLAY.md#penetrator) |
| R08 | Four-line bonuses capped at three; all-white penetrator and one symbol per available bonus | C1/C8, [penetrator](spec/GAMEPLAY.md#penetrator) |
| R09 | Six settings and defined live-change behavior | C6/C7/C8, [schema](spec/CONFIGURATION.md#settings-schema) |
| R10 | Independent URL → cookie → default restoration, including invalid and duplicate inputs | C7, [restoration](spec/CONFIGURATION.md#restoration) |
| R11 | Setting changes synchronize cookies and URL; explicit valid URL defaults persist; omission retains cookies | C7, [writes and failures](spec/CONFIGURATION.md#writes-and-failures) |
| R12 | Readable, keyboard-usable interface, accurate previews/status, nonwhite regular pieces | C6/C8, [presentation](#presentation-and-browser-behavior) |
| R13 | Reproducible static build and isolated Chromium/Firefox acceptance evidence | C6–C8, [delivery qualities](#delivery-qualities) |

## Shared guarantees

A session contains a settled board, active piece, hold slot/use flag, piece-source state/queue, score/lines/level, bonus balance, lifecycle and simulated timers. Settings are validated separately and survive restart. Ghost and preview are derived views. Only legal lock, clear and restart change settled cells. Rejected/disabled inputs are no-ops unless a specified terminal condition ends the game.

Session time advances in integer milliseconds supplied by the controller. Rendering does not change state or consume randomness. The same initial state, seed, settings and ordered time/input events must yield the same game result. Hidden-page or focus-loss time never accrues to gameplay. A timestamp tie processes expired lock before gravity; commands follow their observed order and never target a piece already removed by an earlier event.

## Presentation and browser behavior

Display board, score, total lines, level, game state, controls, settings, next/hold areas and bonus symbols with descriptive HTML labels. Hidden/disabled previews display an explicit disabled/empty state and cannot reveal upcoming or held content accidentally. Regular piece colors must all differ from white; penetrator cells are opaque white. Ghost outlines must be distinguishable from solid active cells and settled cells. Overlapping penetrator traversal must show its active cells visibly while leaving the settled board intact underneath.

Start, pause/resume, restart and all settings are keyboard focusable. Give settings native labeled controls and visible focus. Suppress page scrolling only for recognized game keys while the game surface owns focus; typing/selecting in settings must not move pieces. At narrow widths the side panel may stack, but board and controls remain reachable without clipped content. Re-render correctly after resize without resetting the game. Use text for numeric/status content; Canvas alone is not a screen-reader status channel. Full nonvisual board play and touch input are outside scope.

## Delivery qualities

Produce static build assets and a reproducible dependency lockfile. A clean checkout with documented compatible Node/npm can install, check types, run unit checks, build and serve the product using documented commands. Pin dependencies during implementation; this document does not claim packages or binaries are installed.

Use headless Chromium as primary and managed Firefox as secondary, both isolated under BROWSER_DEV_POLICY.md. Required evidence includes real keyboard input, settings/reload, Canvas piece/ghost/white rendering, lifecycle repetition and teardown on both targets for delivered features. Screenshot inspection or pixel/shape assertions must support visible behavior; DOM-only assertions cannot prove Canvas output. Missing required browser evidence blocks the affected phase acceptance rather than being relabeled optional. No system/personal browser may substitute implicitly.

Persistent-cookie/history errors must preserve a playable session and report that reload may lose the selected value. Invalid external values never crash initialization. No credentials, browser binaries, debug fixtures or test reports belong in the static product.

## End-to-end acceptance

Acceptance combines the child boundary examples with representative integrated play:

- Start, move, rotate, clear rows, progress level, reach game over and restart with settings retained; verify pause/focus loss prevents catch-up.
- Verify deterministic bags and hold consumption; wall-edge kicks, blocked rotations and spawn collision preserve board invariants.
- Exercise both locking policies and both hard-drop policies at exact timer boundaries, with airborne cancellation, grounded movement, pause and live settings changes.
- Compare ghost and actual hard-drop landing in ordinary and penetrator tunnels, including a straddling ghost, no valid landing and invalid terminal overlap.
- Earn/spend capped bonuses, verify white rendering and disabled hold, and ensure traversal never destroys settled cells.
- Independently mix URL, cookie, default and invalid values across six fields; reload after changes, explicit defaults and omitted keys; verify unrelated URL/cookie content survives.
- Run delivered-feature checks in both declared browsers and inspect rendering. Record runtime/version/commands and actual limitations with phase evidence.
