# Gameplay contracts

## Board and pieces

R01: use 10 columns x 20 visible rows, indexed x=0..9 and y=0..19, with four hidden spawn rows y=-4..-1. Stored rows include the hidden area. In-board means every occupied cell is within x=0..9 and y=-4..19; a completed lock must also have every occupied cell visible. Any piece that would lock partly above row 0 ends the game without adding cells. A blocked spawn also ends the game. New sessions contain no settled cells.

A shape is a square local grid. The spawn orientation is defined below (`.` empty, letter occupied). The piece origin is its grid's upper-left cell; spawn x=floor((10-grid width)/2), y=-1. Rotation clockwise maps local (x,y) to (n-1-y,x); counterclockwise applies its inverse. Rotation retains the origin before kick tests. O is unchanged by rotation. Occupied cells, not empty grid padding, determine boundaries and collision.

| Type | Grid width | Spawn rows separated by `/` |
| --- | --- | --- |
| I | 4 | `..../IIII/..../....` |
| O | 2 | `OO/OO` |
| T | 3 | `.T./TTT/...` |
| J | 3 | `J../JJJ/...` |
| L | 3 | `..L/LLL/...` |
| S | 3 | `.SS/SS./...` |
| Z | 3 | `ZZ./.ZZ/...` |

Wall kicks use these project-defined candidate offsets in order, for either rotation direction and all types except O: (0,0), (-1,0), (1,0), (-2,0), (2,0), (0,-1). Accept the first permitted placement; otherwise rotation is a no-op. This deterministic kick system is not a claim of SRS conformance. Rotation/movement of a regular piece cannot overlap settled cells; a penetrator uses the traversal rule below.

A legal lock adds four cells atomically, then removes all full visible rows simultaneously, shifts rows above down (including hidden rows), fills the vacated hidden rows with empty cells and updates progression. Never clear a partial row or score a traversed occupied cell. If the active piece would be invalid at lock, end the game without a partial write.

Acceptance: spawn all types; rotate through four orientations; test each board edge/hidden boundary and total kick failure; clear 1/2/3/4 rows simultaneously with occupied hidden rows; confirm exactly four active cells and settled-cell conservation except cleared rows.

## Lifecycle and input

R02: states are ready, running, paused and game-over. Start enters a fresh running session; restart replaces any session and begins running. Ready/game-over reject gameplay commands. Pause/resume applies only to a running/paused session. Blur or document hidden pauses running play, clears held keys and requires explicit resume. Restart resets score, board, bonuses, hold, sequence and timers while retaining settings.

| Control | Action |
| --- | --- |
| Left / Right arrows | Move one column |
| Down arrow | Soft drop one row |
| Up arrow or X | Rotate clockwise |
| Z | Rotate counterclockwise |
| Space | Hard drop if enabled |
| C or Shift | Hold if enabled and eligible |
| P or Escape | Pause/resume |
| B | Activate penetrator if eligible |

Start/restart use labeled buttons to avoid accidental destructive hotkeys. All bindings apply only when the game surface owns focus, except loss of focus itself. Native key auto-repeat is ignored. Left/right perform one immediate action, then repeat after 150 ms at 50 ms intervals; latest pressed horizontal direction wins until released, then the other held direction resumes with a fresh repeat delay. Down acts immediately and repeats every 50 ms. Other game commands require a fresh keydown; releasing a key creates no move. Pause clears repeats; resume never replays stale held keys.

## Progression

R03: score begins at 0, lines at 0 and level at 1. Each lock's clear scores 0/100/300/500/800 for 0/1/2/3/4 lines, multiplied by the level before that clear. Add successful soft-drop distance at 1 point/row and hard-drop distance at 2 points/row; automatic gravity and failed moves award none. After clearing, level=1+floor(total lines/10). Gravity interval=max(100, floor(1000 x 0.85^(level-1))) milliseconds. A lock timer retains its captured interval even if the level changes through a future session transition. There are no combo, spin, back-to-back or perfect-clear bonuses.

Acceptance: verify each clear score, a 9→10-line level boundary, level-1 interval 1000 ms, level-2 interval 850 ms and minimum-speed clamp. Drop score is awarded once per actual row, including penetrator traversal.

## Gravity and locking

R04: gravity advances one row per complete interval while running. Reset gravity progress on spawn, hold replacement and completed hard drop; lateral movement, rotation and failed downward commands do not reset it. Successful soft drop resets gravity progress, preventing simultaneous double fall. A new session/spawn starts with zero progress.

For regular pieces, grounded means current placement is legal and one row below is illegal. For penetrators, grounded means current placement is legal and no deeper legal placement exists at the same x/orientation. Evaluate grounding after every successful movement, rotation, hard drop, activation, spawn/hold and relevant settings change, and before processing time.

Under interval policy, capture one current gravity interval on first grounding. Lock when that much running simulated time has elapsed, including exact equality. Grounded movement/rotation does not refresh it. Becoming airborne cancels it; later grounding captures a full new interval. Failed downward movement cannot shorten it. If a spawn is grounded, its timer starts immediately. Pause freezes gravity, lock and input-repeat progress.

Under failed-down policy, lock only on a failed gravity or soft-drop descent when the current piece is legally grounded. A move that first reaches grounding does not lock. A hard drop following this policy includes its terminal failed descent and locks even when distance is zero. Immediate hard-drop policy locks a legal landing at once under either general policy. Following interval policy preserves a running timer or begins one for a newly grounded landing, including a zero-distance drop.

An expired timer is processed before a gravity event at the same simulated timestamp. Lock/clear/spawn occur once; any remaining elapsed time applies to the new piece starting at the spawn timestamp. Equal event timelines yield equal states regardless of render-frame partitioning. A live general-policy change cancels prior lock progress: interval starts fresh if grounded; failed-down waits for a new attempt. Reapplying the same policy is a no-op. Changing hard-drop policy affects subsequent hard drops only.

Acceptance: at 999 ms grounded under level 1 do not lock; at 1000 ms lock once. Grounded shifts/rotations and repeated hard drops cannot restart that timer. Airborne motion cancels it; re-grounding creates a full interval. Pause for arbitrary wall time leaves timers unchanged. Check same-time gravity/lock, large elapsed steps versus equivalent partitions and policy changes.

## Sequence and hold

R05: each bag is a permutation of I/O/T/J/L/S/Z with each type exactly once; refill only after consuming seven. A seeded source is reproducible. Maintain the active piece and enough queued types to show exactly one next piece when enabled; hiding preview changes no draws or queue order. A new session has an empty hold slot and hold available.

An eligible hold stores the outgoing type and either consumes the next queued type for an empty slot or exchanges the stored type without consuming the queue. Replacement uses spawn orientation/location, zero gravity/lock progress and normal spawn validation. After a successful hold, further hold is disabled until a piece locks and a new piece spawns. Blocked replacement causes game-over. Toggle hold off retains the stored type/use state; toggling on grants no additional hold. Active penetrator hold is always rejected without spending a bonus or changing the queue.

## Landing and ghost

R06: regular landing is the deepest legal position reached by consecutive downward steps without crossing an illegal placement. Hard drop moves there and awards distance once. Disabled hard drop is a complete no-op. Ghost uses precisely that same landing function without consuming randomness, score or time. Dynamic mode hides if the minimum occupied ghost y is less than 10; straddling y=9/10 is hidden, minimum y=10 is shown. Always hidden never draws it; always shown draws a valid projection even in the upper half. No valid projection draws no ghost. A zero-distance ghost may be omitted where active cells fully cover it; this cannot hide other cells or change the landing result.

Acceptance: compare ghost occupied coordinates with a hard-drop result before lock, at edges, beneath overhangs, at min-y 9/10 and for hidden-row positions. Repeated redraws/ghost queries must leave source state, timers and board unchanged.

## Penetrator

R07/R08: a four-line clear earns exactly one bonus, capped at three. Every new session begins with zero. A running, non-penetrator active piece with balance>0 can activate once with B, immediately spending one bonus. Activation is one-way until lock; repeated B is a no-op and spends nothing. The white active piece may move/rotate through occupied cells, retaining occupied-cell board bounds and the same ordered kick offsets. It cannot be held. Its ability never transfers to the next piece or held slot and spent bonuses are not refunded on game-over/restart.

For current x/orientation, enumerate in-board nonoverlapping placements at y at least the current y; the maximum y is the penetrator landing. Intervening overlaps do not block reaching it. Hard drop and ghost use exactly this query; hard drop never moves upward. If none exists, ghost is absent and hard drop is a no-op (including immediate mode). Normal descent may traverse overlaps, but when the current legal placement is the deepest legal one, it is grounded and downward attempts fail even if the next row would permit overlapping traversal. This prevents passing below the last legal landing. If no legal placement remains and downward traversal reaches the board bottom while overlapping, end the game without changing settled cells.

Overlapping traversal never starts a lock timer. Horizontal/rotation changes recompute the landing. Existing grounding time is preserved if the piece remains legally grounded; becoming airborne cancels it. Activating on a regular grounded piece cancels its timer if a deeper penetrator landing becomes available; otherwise preserve the timer. Any attempted lock must revalidate visible, in-bounds, nonoverlapping cells. Bonus acquisition occurs after clear scoring and before next spawn. Display exactly one lightning symbol per available bonus, immediately reflecting activation/clear; balance cannot go below 0 or above 3.

Acceptance: tunnel through a blocking row to a lower cavity, compare ghost/hard drop, and verify original cells unchanged; stop at a deepest legal landing above overlapping bottom cells; handle no legal landing without upward teleport or illegal lock; reject hold and repeated activation; verify balances 0→1, 2→3, capped 3→3 and spending 1→0, plus white piece rendering and nonwhite ordinary pieces.
