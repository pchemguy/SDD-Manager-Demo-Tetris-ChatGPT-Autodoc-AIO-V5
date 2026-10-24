# Browser Tetris

## Purpose and users

Build a single-player browser Tetris game for keyboard players who want a familiar falling-block game with optional modern aids and a penetrator mechanic. Deliver a simple playable classic game first, then useful bounded increments until the complete scope below is available.

## Intended outcome

Players can start, pause, resume and restart a game; move and rotate falling tetrominoes; clear lines; and see score, cleared lines, level and game-over state. Rules remain predictable across frame rates, and controls and configuration remain understandable alongside the board.

## Complete scope

- Classic falling tetrominoes, movement, rotation, gravity, locking, line clearing, scoring, increasing level/speed and game over.
- Hold, ghost projection, seven-bag randomization, wall kicks and hard drop.
- A default lock delay of one full gravity interval after becoming grounded; hard drop follows the general policy unless immediate locking is selected.
- Three ghost visibility modes: hidden, dynamic above-midline hiding, and always shown.
- A hotkey-activated penetrator that passes through occupied cells without removing them, lasts until its piece locks, and locks only entirely inside the board without overlap. Its hard drop and ghost share the lowest valid landing position.
- One penetrator bonus per four-line clear, a maximum balance of three, white penetrator pieces, nonwhite regular pieces and one side-panel symbol per available bonus.
- Side-panel controls for ghost visibility, hard drop, next preview, hard-drop locking, general locking and hold.
- Independent per-setting restoration from valid explicit URL value, then valid cookie, then default. Changes update both the cookie and URL. Valid explicit URL values also update cookies, even when equal to defaults; absent parameters preserve saved values. Session-only settings may precede persistence.

## Proposed interaction choices

Approval adopts the following proposals, with rationale and boundary details in [the preparation decisions](DOCS_PREPARATION.md#proposed-gameplay-decisions-for-approval):

- A ghost straddling the midline is hidden; show it only when its entire shape lies in the lower half.
- Grounded movement and rotation do not reset lock delay. Becoming airborne cancels the timer; later grounding begins a full interval. Pause freezes timing.
- Under failed-down locking, hard drop follows that policy by counting its terminal failed descent as an attempt. Under interval locking, hard drop preserves an existing timer or starts one on new grounding.
- Penetrator activation spends one bonus until lock and disables hold. Repeated activation cannot spend another bonus. Traversal never destroys settled cells; locking requires a valid deepest landing. Impossible terminal overlap ends the game without overwriting the board.
- Setting changes apply during play; a general locking-policy change starts a fresh interval when legally grounded, or waits for the next downward attempt under failed-down policy.
- Invalid settings fall through independently to the next valid source and never overwrite a valid cookie. Unrelated URL parameters are preserved.
- Default settings enable dynamic ghost, hard drop, next preview and hold, with full-interval general locking and hard drop following it.

## Technical direction and constraints

Proposed direction: TypeScript with a Canvas board and ordinary HTML controls, a deterministic gameplay core independent of rendering, and a static distributable build. Canvas suits the grid and ghost/penetrator visuals; native controls make settings usable without custom interaction widgets. A DOM-rendered board is a credible alternative but offers little benefit for the compact grid; a larger UI framework would add machinery to a small application. The detailed toolchain remains an architecture decision.

Use keyboard-first desktop interaction, responsive layout, visible focus, readable controls and instructions, and distinguishable regular-piece colors. No backend is required. Browser verification prioritizes isolated Chromium and includes Firefox under the repository browser policy. The execution host may be Linux; Windows-specific shell rules apply only on Windows hosts.

## Delivery and release constraints

The first increment must be a playable classic baseline rather than a container for all enhancements. Subsequent phases add coherent capabilities and preserve existing play. Use semantic versioning, beginning with a proposed baseline release v0.1.0 and increasing versions for delivered increments. Publish a GitHub release with a version tag and release notes only after each implementation phase has been completed, verified and integrated into the actual default branch. No release is justified by documents alone.

## Proposed exclusions

Multiplayer, accounts, network leaderboards, mobile touch controls, audio, saved in-progress games and persistent scores are outside the target. Persistence covers configuration only. These exclusions keep the initial project focused and may be revised before approval.

## Remaining design work

Architecture and specification will define exact board/spawn geometry, rotation kick tables, keyboard bindings, scoring, gravity progression, penetrator intermediate-state timing, configuration encodings and cookie attributes. These details must preserve the approved interactions above. No implementation exists yet.
