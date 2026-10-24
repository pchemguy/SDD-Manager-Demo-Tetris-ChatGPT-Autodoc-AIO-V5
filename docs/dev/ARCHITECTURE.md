# Architecture

## System context

The [project brief](PROJECT.md) defines a keyboard-first, single-player browser game. The application is a static client: the browser owns a live game session and six user settings. Cookies and the current page URL are the only persistent interfaces. No server, account or runtime network service participates in play.

## Major blocks

| Block | Responsibility and owned state | Dependencies |
| --- | --- | --- |
| Gameplay core | Board, active/held/queued pieces, lifecycle, score, level, bonuses, randomness cursor and simulated timers | Pure geometry/rules and an injected piece source; no browser APIs |
| Application controller | Serialize commands and elapsed time, adapt input, pause on focus loss, select effective settings and publish snapshots | Gameplay core and browser adapters |
| Presentation | Canvas board and piece previews; HTML statistics, instructions, settings and lifecycle controls | Read-only snapshot and controller commands |
| Browser adapters | Keyboard events, animation clock, cookies and URL/history writes | Browser interfaces; injected ports for deterministic checks |

The controller is the single live-session writer. The gameplay core transforms an explicit state and command/time event into a new state and outcomes. Rendering consumes snapshots and cannot mutate the board. Random draws belong to the piece-source state, never the renderer or ghost calculator.

## Dependency direction

Presentation and browser events call the controller. The controller calls the gameplay core and settings adapters. The core calls pure geometry, rotation and landing rules. Geometry and landing never depend on the controller, persistence, Canvas or DOM. Configuration decoding is pure; storage and URL mutation wrap it at the browser boundary. This directed arrangement permits gameplay and configuration precedence checks without a browser.

## Important interactions

- Keyboard commands and clock events enter one ordered stream. Pause freezes simulated time; resume never applies the real-time gap. Restart replaces the session while retaining effective settings.
- Regular falling, hard drop and ghost projection share one landing calculation. Penetrator projection uses a traversal policy over the same geometry and board snapshot, searching for the deepest legal placement. Rendering never guesses a second landing.
- Locking alone commits active cells to the board. The core rejects overlap/out-of-bounds writes, clears rows atomically, updates score/level and bonuses, and spawns the next piece. Hold exchanges types through the same spawn validation and cannot preserve penetrator state.
- The settings boundary resolves each field independently before session creation. Changes update the controller immediately and attempt cookie/history synchronization. Storage failure cannot corrupt a running session; a visible status explains a failed persistence attempt.
- Derived views include ghost visibility, bonus symbols, previews and enabled controls. A snapshot is internally consistent with its originating game state and settings.

## Technology and tradeoffs

Use TypeScript, a Canvas board and native HTML controls. Select Vite for development/static bundling, Vitest for deterministic unit/integration checks and Playwright Test for application checks. Pin exact compatible versions and a lockfile when implementation begins; document preparation creates no package installation. A framework or component/event-bus architecture adds little value to this small interface, while native controls preserve keyboard accessibility.

Inject time and seeded random input for reproducibility. Keep gameplay data serializable for fixtures, but do not expose a production save-game feature. A narrow test harness may supply fixtures without becoming a supported public API. Prefer cohesive rule functions and a small controller over a class hierarchy per tetromino or a generic plugin framework.

## Architectural invariants and failure boundaries

- Settled cells change only during legal lock/clear or session replacement. Traversing pieces never erase them.
- A pending timer is explicit session state. Browser wall time and render frequency cannot independently trigger a second lock.
- Piece generation is deterministic for an explicit source state. Projection and redraw consume no random pieces.
- Cookie/history errors are isolated at their adapters; invalid external values never reach the core as unvalidated settings.
- Chromium is the primary automated browser and managed Firefox the secondary target. Dedicated binaries/profiles and finite process lifetimes follow the root browser policy; unavailable required capability blocks its acceptance claim.
- Static build assets and verification artifacts are separate; no credentials, browser binaries or test fixtures ship accidentally.

DECOMPOSITION refines logical components. Exact public behavior belongs to SPEC, delivery strategy to PLAN, and physical placement to layout. No focused architecture children are needed at this scale.
