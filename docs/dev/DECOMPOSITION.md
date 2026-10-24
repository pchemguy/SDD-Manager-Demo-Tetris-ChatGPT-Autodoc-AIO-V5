# Decomposition

## Component model

The [architecture](ARCHITECTURE.md) separates the gameplay core from a thin controller, presentation and browser adapters. Components below are logical ownership boundaries, not a demand for one class or file per row.

| ID / component | Provides / owns | Requires / excludes | Verification seam |
| --- | --- | --- | --- |
| C1 Session transitions | New game, pause/resume, restart, ordered commands, active/held pieces, score/level/bonus state, atomic lock/clear/spawn | C2–C5 and validated settings; excludes DOM, cookies and wall-clock reads | Explicit state + action + elapsed duration yields inspectable state/outcomes |
| C2 Board and piece geometry | Board bounds, occupied-cell coordinates, seven tetromino orientations, placement tests, row compaction and rotation kicks | Board and piece values; excludes score, input and random draws | Table fixtures for edges, rotation orientation and row invariants |
| C3 Piece source | Reproducible piece sequence, seven-bag permutation/cursor and queue replenishment | Injected random source/state; excludes hold/ghost/rendering | Fixed seeds/draws verify bags, queue order and consumption |
| C4 Landing and visibility | Regular/penetrator landing queries and dynamic ghost visibility from a snapshot | C2 and active piece/board/settings; excludes board mutation and randomness | Deep-stack, straddling-midline, tunnel and no-landing fixtures |
| C5 Timing rules | Gravity progress, grounded interval capture/cancellation, pause-safe time and one lock decision | Simulated elapsed time and placement/landing facts; C1 owns resulting state | Boundary timestamps, frame partitioning and airborne transitions |
| C6 Controller and input | Own live session, command serialization, focus lifecycle, key repeat and effective setting application | C1, C7, browser event/time ports; excludes duplicated gameplay rules | Fake event/time ports and real-browser keyboard checks |
| C7 Settings and persistence | Typed six-field schema, validation/defaults/precedence, cookie and query encoding, safe writes/status | Cookie/history ports; excludes active-piece mutation and saved games | Pure precedence matrices plus adapter failure and reload tests |
| C8 View and controls | Canvas board/ghost/previews, HTML statistics/bonuses/settings, control availability and status | Read-only snapshots, C6 command callbacks; excludes random draws and legality decisions | Snapshot checks plus real Canvas/glyph/keyboard inspection |

## State and collaboration

C1 owns the authoritative session value. Board cells contain tetromino identities, not presentation colors; active-piece penetrator state and bonus balance are separate fields. C3 source state and C5 timer values belong to that session and advance only through C1. C4 returns derived geometry without caching mutable references across transitions. C8 maps identities to nonwhite colors and active penetrator state to white.

C6 owns the session reference and current validated settings. One callback serializes inputs, simulates time and emits one coherent snapshot. C6 passes settings changes through C1 for timer/input consequences, then asks C7 to synchronize external representations. C7 returns per-operation success/failure; C6 exposes a persistence status without rolling back valid session settings.

The core uses a closed action set for lifecycle, movement, rotation, soft/hard drop, hold, penetrator activation and setting application. Detailed names and data representations remain implementation choices. Consumers can rely on total handling of permitted commands: blocked/disabled actions have a defined no-op or game-over outcome in SPEC, never partial board writes.

## Lifecycle and invariants

- Spawn and hold replacement use the same C2 spawn geometry and collision validation. Hold stores only a tetromino type; it cannot carry a penetrator flag or a lock timer.
- C1 asks C4 for a landing before ghost display or hard drop. C5 receives the same grounding predicate; it cannot start a lock timer during overlapping penetrator traversal.
- Lock commits only after C2 legal-placement validation, then clears rows, scores and updates bonuses before the next spawn. C3 supplies pieces only at actual spawn/queue-consumption boundaries.
- C6 creates listeners/animation callbacks once and removes them on teardown. Restart replaces state instead of installing another loop. Focus loss pauses rather than replaying held keys on return.
- C8 disables controls when unavailable and displays the effective setting even if external persistence fails. C7 never overwrites unrelated cookie names, URL keys or fragments.

## Dependency and cohesion assessment

C1 coordinates pure rule helpers; none calls back into C1. C6 orchestrates side effects; C8 submits commands through callbacks and reads snapshots. C7's pure codec and browser ports share one settings responsibility but can be checked separately. No component needs a global event bus or a dependency container. Tests should use public transition/port boundaries rather than mirror private helper implementation.

SPEC is the canonical owner of precise behavioral contracts and acceptance, including rotation tables, timing, failure outcomes and external encodings. Physical ownership belongs to layout. No independent child documents or additional public APIs are required.
