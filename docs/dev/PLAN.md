# Delivery plan

## Objective and strategy

Deliver the [specified game](SPEC.md) through five useful releases. Each phase preserves its predecessor and adds a coherent capability. The classic baseline is a complete playable game; modern aids, configuration, penetrator behavior and persistence are subsequent increments. The static client and pure-rule boundaries follow ARCHITECTURE and DECOMPOSITION.

## Phase map and contract coverage

| Phase | Useful result / proposed version | Included contracts | Explicit deferrals |
| --- | --- | --- | --- |
| 1 — Classic game | Playable keyboard Tetris, v0.1.0 | R01 basic rotation without kicks; R02–R03; R04 interval locking; relevant R12–R13 | Seven-bag, hold, kicks, hard drop, ghost, settings and penetrator. Baseline piece draws may be independent uniform random types; no seven-bag claim. |
| 2 — Modern piece handling | More predictable and controllable play, v0.2.0 | R01 wall kicks; R05 seven-bag/hold/next preview; R06 hard drop; R04 hard drop follows interval delay; relevant R12–R13 | Ghost and policy/configuration controls; penetrator; persistence. Hard drop, hold and next preview are enabled fixed capabilities here. |
| 3 — Configurable play aids | Ghost and all six live session settings, v0.3.0 | R04 both general/hard-drop policies; R06 complete ghost/midline; R09 session controls; relevant R12–R13 | Reload persistence R10–R11 and penetrator. Defaults reapply on reload in this explicit session-only increment. |
| 4 — Penetrator game | Earn/spend bonuses and tunnel through stacks, v0.4.0 | R07–R08 plus penetrator interactions with R04–R06/R09/R12–R13 | URL/cookie persistence only. |
| 5 — Persistent complete game | Portable/restored configuration and full target, v1.0.0 | R10–R11 plus complete R01–R13 regressions | Only the accepted project non-goals. |

Versions advance for delivered scope and are recorded in package metadata and release notes. An unplanned compatible fix uses a patch increment; do not tag a failed or partial phase. The planned major 1.0 release denotes completion of the accepted target, not a claim of future compatibility with unrelated Tetris variants.

## Phase 1 — Classic game

Outcome: start, move, rotate, fall, clear lines, score, level up, pause/restart and reach game-over in a static browser app. Full-interval locking is essential baseline behavior, not deferred. Uniform draws and no-kick rotation keep the first product focused without designing a second engine.

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 1.1 — Classic rules | Deterministic board/shape/session/timing/clear progression produces legal states; fixtures cover spawn failure, edges, hidden rows, lock equality and multirow clears. This is an enabling core, not a playable-product claim. |
| 1.2 — Playable browser | Controller, keyboard and Canvas/HTML provide the complete classic use path with readable status, pause/focus behavior and restart. Demonstrate representative play and inspect rendering to decide whether controls/board need amendment before growth. |
| 1.3 — Verified static baseline | Pin toolchain/build, provision isolated Chromium/Firefox and check actual application interaction/rendering plus repeat lifecycle/teardown. Documentation and static archive/version/notes preparation describe only baseline scope. |
| 1.4 — Phase review | Cross-milestone code review, required checks/repairs and phase report establish the baseline exits before integration/release. |

Depends on accepted documentation integration and authorized implementation activation. Exit: playable baseline and relevant R01–R04/R12–R13 checks pass on both browsers, build/type/unit checks pass, milestone reports exist and phase review is complete. No later-feature check is represented as satisfied.

## Phase 2 — Modern piece handling

Depends on phase 1 verified, integrated and released. Seven-bag/hold and wall kicks/hard drop share piece lifecycle boundaries but do not require configurable policies or persistence.

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 2.1 — Queue and hold | Seven-bag source, exactly one next preview and once-per-lock hold integrate with spawn/game-over. Verify deterministic bag/queue consumption and disabled-repeat hold; inspect preview/hold feedback. |
| 2.2 — Kicks and hard drop | Ordered kick tests and hard drop use shared placement/landing rules; full-interval locking follows hard drop without resetting a running timer. Check wall/stack edges, zero-distance drops and score. |
| 2.3 — Modern play verification | Keyboard/view integration, type/unit/build and both-browser regressions demonstrate the complete changed experience. Update controls/docs/version and release package/notes preparation. |
| 2.4 — Phase review | Review cross-feature spawn/hold/drop/timer interactions, required regressions, blockers and phase report. |

Exit: modern controls preserve baseline invariants and shared source consumption; browser evidence covers new previews, keyboard actions and delayed hard drop. Deferring independent settings/ghost avoids an oversized second phase.

## Phase 3 — Configurable play aids

Depends on phase 2 completion/integration/release. Ghost reuses hard-drop landing, and settings make its visibility and existing actions configurable. Both locking alternatives belong here because their controls form one complete settings panel.

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 3.1 — Ghost visibility | Shared landing projection and all visibility modes handle upper, straddling and lower positions without state mutation. Inspect ghost outlines against active/settled pieces and compare with hard drop. |
| 3.2 — Session settings and lock policies | Typed schema and six controls apply immediately; failed-down/immediate/follow/interval combinations and live changes have deterministic timing. Hold/preview hiding preserves slots and sequence. |
| 3.3 — Configurable experience verification | Real-browser settings/input focus, paused edits, state preservation, resize and both-browser regression evidence; documentation explicitly describes session-only settings, with package/version/notes preparation. |
| 3.4 — Phase review | Review all settings and timing/visibility interactions, required checks/repairs and phase report. |

Exit: all six settings and ghost modes work during play; no persistence is claimed. Representative panel/keyboard use informs whether labels or focus need amendment before the penetrator interaction matrix grows.

## Phase 4 — Penetrator game

Depends on phase 3 completion/integration/release. Activation requires bonus/scoring state; tunneling requires the established projection and timing boundaries; view/configuration integration verifies hold suppression and white rendering.

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 4.1 — Bonus activation | Four-line awards, capped balance and one-way activation/spending integrate with hold prohibition and session replacement. Verify cap/no-refund/repeated-activation cases and immediate symbol updates. |
| 4.2 — Traversal and legal landing | Movement/rotation traversal, deepest legal projection/drop and grounded/timer/terminal-failure rules preserve settled cells. Fixtures include bottom overlaps, hidden rows, cavities, no legal landing and no upward drop. |
| 4.3 — Penetrator experience verification | White rendering, bonus glyphs and settings/hold/drop combinations are exercised in both browsers; regress classic/modern play and prepare docs/package/version/notes. |
| 4.4 — Phase review | Review activation/traversal/timing/projection interactions, all phase exits, blocker repairs and phase report. |

Exit: no overlapping lock or destroyed settled cell, ghost/drop agree, balance and hold rules remain consistent under both locking policies, and rendered evidence supports the visible result. This phase adds gameplay only; persistence remains independent.

## Phase 5 — Persistent complete game

Depends on phase 4 completion/integration/release for release sequence, and consumes phase 3's stable settings schema. Keeping persistence last isolates external storage/URL failure from the earlier playable increments.

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 5.1 — Settings restoration | Canonical codecs and independent URL/cookie/default resolution cover every field, invalid/duplicate/omitted/explicit-default inputs and startup cookie synchronization. |
| 5.2 — Persistence adapters | Cookie/history writes preserve unrelated data, update only the changed field, and expose independent nonblocking failures; real reloads verify success cases. |
| 5.3 — Complete product verification | Full target matrix in Chromium/Firefox, static package inspection, clean-install/build instructions and final version/release-message preparation. Demonstrate portable URL settings and retained cookie defaults to confirm the complete user outcome. |
| 5.4 — Phase review | Full-range code review, required checks/repairs, phase report, final implementation report and TODO aggregation. |

Exit: R01–R13 and child acceptance are covered by appropriate current evidence, no blocking findings remain, and the final artifact contains only intended static files. Cookie/history denial has truthful limits and never blocks play.

## Review, integration and release gates

Every delivery milestone requires whole-capability code review, relevant testing/regressions, blocker repair, a committed milestone report and HANDOFF. TASKS assigns a standalone review task only with at least four implementation tasks in that milestone and at least two implementation milestones in its phase; otherwise its final implementation task owns that evidence. Every phase has the dedicated final review milestone above with exactly one code review/testing/report task, dependent on completion and hosted closure of all delivery milestones.

Each phase exit includes build/type/unit and appropriate both-browser evidence, truthful docs, version/package/notes preparation and preserved prior behavior. Required browser provisioning/launch/session/rendering failure blocks its acceptance until resolved under the browser policy; do not silently drop Firefox or use a personal installation. Gather representative usability/functional evidence at the stated milestones; these are evidence gates, not additional human approval requests inside an already authorized range.

The coordinator integrates only complete verified phases into the actual default branch using an explicit two-parent merge, verifies the merged state and publishes it. After verified integration, finalize the exact release message/version, create an eligible default-branch release commit, publish/read back its tag and publish/read back the GitHub release and intended assets. The user's release instruction authorizes these phase releases when implementation is selected; preparation itself creates none. A failed push/release retains pending effects and blocks the next phase's dependent progression. No phase is complete solely because its task checkboxes are checked.

## Risks and boundaries

- Timing and penetrator traversal are the principal correctness risks; deterministic fixtures precede browser integration.
- Canvas output needs direct rendering evidence; DOM counts alone cannot close visible acceptance.
- Browser provisioning is verified before dependent UI checks. A working install does not prove page creation/interaction/teardown.
- External cookie/history restrictions require explicit limits and regression checks, not a backend workaround.
- TASKS derives executable scopes; layout allocates files. Neither count preferences nor eventual version scope broadens the classic baseline.
