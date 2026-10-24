# Implementation tasks

Derived from [PLAN](PLAN.md), [SPEC](SPEC.md), [DECOMPOSITION](DECOMPOSITION.md) and [layout](layout.md). All work is planned and unchecked. Each task owns its changed result, meaningful verification and affected documentation.

Implement only an authorized range. Every task completion commits its verified result/evidence and initial checkbox together, pushes and reads back before the next task, then closes its issue. Milestone/phase closures and integration follow PLAN; no checked task alone establishes a release. Activate only the eligible phase, after required preparation integration and predecessor integration/release.

Report paths below are intended outputs, not existing evidence. Milestone/phase reports include Findings/Blockers and TODO (None when empty), preserving disposition and provenance. Folded milestone exits include code review, focused tests/regressions, blocker repair, report and HANDOFF in that final implementation task’s completion commit.

## Phase 1 — Classic game

- [ ] Phase 1 — Classic game
    - [ ] Milestone 1.1 — Classic rules
        - [ ] T-001 — Define board and tetromino geometry
            Scope: src/game/types.ts, pieces.ts, geometry.ts; tests/game/geometry.test.ts; initial package/lock/type/unit configuration.
            Depends on: none; accepted documentation integration and phase 1 activation are prerequisites. Contracts: R01 in SPEC.
            Evidence: Establish pinned compatible TypeScript/unit tooling and validated check commands; shape cells, visible/hidden bounds, four orientations, placement rejection and baseline no-kick rotation are deterministic.
        - [ ] T-002 — Implement classic session transitions
            Scope: src/game/session.ts, source.ts; tests/game/session.test.ts.
            Depends on: T-001. Contracts: R01–R02 in SPEC.
            Evidence: Start/restart, active spawn, movement, blocked commands and spawn game-over preserve board invariants; independent uniform source is explicit baseline scope.
        - [ ] T-003 — Implement gravity and interval locking
            Scope: src/game/timing.ts, session.ts; tests/game/timing.test.ts.
            Depends on: T-002. Contracts: R02/R04 in SPEC.
            Evidence: One captured interval from grounding, equality, airborne cancellation, no grounded reset, pause and ordered time-step partitions pass.
        - [ ] T-004 — Implement atomic clears and progression
            Scope: src/game/geometry.ts, session.ts; tests/game/session.test.ts.
            Depends on: T-003. Contracts: R01/R03 in SPEC.
            Evidence: Legal visible lock, 1–4-line compaction, hidden-row movement, scoring/level/speed and invalid lock failure pass without partial writes.
        - [ ] T-005 — Review, test and report milestone 1.1
            Depends on: T-001, T-002, T-003, T-004.
            Scope: whole 1.1 implementation and relevant dependencies, required focused tests/regressions and blocker repairs.
            Evidence: milestone exits satisfied; report with finding/TODO dispositions and HANDOFF committed with this task completion. Report: docs/dev/reports/phases/1/1.1.md.
    - [ ] Milestone 1.2 — Playable browser
        - [ ] T-006 — Provision the isolated browser development environment
            Scope: package/lock/build/browser configuration, scripts/browser-env.mjs and generated-path ignore rules.
            Depends on: T-005. Contracts: R13 in SPEC.
            Evidence: Before dependent browser work, pin compatible Vite/Playwright, establish the dedicated absolute binary path and policy-compliant Chromium/Firefox preflight through repeated isolated page/interaction/render/teardown cycles with finite process deadlines. Synthetic capability evidence does not claim application acceptance.
        - [ ] T-007 — Compose the browser session controller
            Scope: src/app/controller.ts, clock.ts, src/main.ts; tests/app/.
            Depends on: T-006. Contracts: R02/R12 in SPEC.
            Evidence: One loop/session owns time and snapshots; restart and teardown do not duplicate listeners or timers.
        - [ ] T-008 — Render the classic board and status
            Scope: src/ui/render.ts, styles.css, index.html; tests/ui/.
            Depends on: T-007. Contracts: R12 in SPEC.
            Evidence: Canvas displays nonwhite active/settled cells and responsive board; HTML score/lines/level/state and lifecycle buttons reflect one snapshot.
        - [ ] T-009 — Integrate keyboard and focus lifecycle
            Scope: src/app/input.ts, controller.ts, src/ui/controls.ts; tests/app/ and tests/e2e/.
            Depends on: T-008. Contracts: R02/R12 in SPEC.
            Evidence: Bounded key repeats, recognized-key scroll suppression, focused controls, blur/hidden pause, explicit resume and restart work through actual browser input.
        - [ ] T-010 — Review, test and report milestone 1.2
            Depends on: T-006, T-007, T-008, T-009.
            Scope: whole 1.2 implementation and relevant dependencies, required focused tests/regressions and blocker repairs.
            Evidence: milestone exits satisfied; report with finding/TODO dispositions and HANDOFF committed with this task completion. Report: docs/dev/reports/phases/1/1.2.md.
    - [ ] Milestone 1.3 — Verified static baseline
        - [ ] T-011 — Verify reproducible browser and build checks
            Scope: package/lock/config files, scripts/browser-env.mjs, tests/e2e/, .github/workflows/ci.yml.
            Depends on: T-010. Contracts: R13 in SPEC.
            Evidence: Validate the pinned clean-checkout install and CI path; type/unit/build checks, repeatable policy-compliant browser provisioning and actual baseline app interaction/rendering/teardown checks pass. Diagnose supported fallbacks without weakening required coverage.
        - [ ] T-012 — Prepare the baseline static distribution
            Scope: scripts/package.mjs, README.md, RELEASE-NOTES.md, package metadata and generated-path ignore rules.
            Depends on: T-011. Contracts: R12–R13 in SPEC.
            Evidence: Document validated setup/controls/limits, prepare v0.1.0 assets/manifest/checksum and complete release message, inspect package contents and rerun baseline checks. Publication follows verified phase integration.
            Milestone exit: whole 1.3 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/1/1.3.md.
    - [ ] Milestone 1.4 — Phase review
        - [ ] T-013 — Review, test and report phase 1
            Depends on: T-005, T-010, T-012 and completion/verified hosted closure of delivery milestones 1.1–1.3.
            Scope: cross-milestone code review, phase exits, relevant prior regressions, build/types/unit and required Chromium/Firefox checks; repair blockers and verify prepared version/package/complete notes.
            Evidence: phase report and HANDOFF identify actual checks/limits and integration/release readiness; no claim of future merge/tag/publication. Report: docs/dev/reports/phases/1/PHASE-REPORT.md.

## Phase 2 — Modern piece handling

- [ ] Phase 2 — Modern piece handling
    - [ ] Milestone 2.1 — Queue and hold
        - [ ] T-014 — Implement the seven-bag source
            Scope: src/game/source.ts, types.ts, session.ts; tests/game/source.test.ts.
            Depends on: T-013. Contracts: R05 in SPEC.
            Evidence: Every seeded bag contains seven unique types; queue replenishment and redraw-independent consumption are reproducible.
        - [ ] T-015 — Implement hold replacement and eligibility
            Scope: src/game/session.ts; tests/game/session.test.ts.
            Depends on: T-014. Contracts: R05 in SPEC.
            Evidence: Empty/nonempty hold consumption, once-per-lock gating, reset orientation/timers and replacement spawn failure pass.
        - [ ] T-016 — Integrate hold and next views
            Scope: src/ui/render.ts, controls.ts, src/app/input.ts; tests/e2e/.
            Depends on: T-015. Contracts: R05/R12 in SPEC.
            Evidence: Exactly one next type and the stored hold type render correctly; C/Shift acts once and live preview/rendering cannot consume pieces.
            Milestone exit: whole 2.1 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/2/2.1.md.
    - [ ] Milestone 2.2 — Kicks and hard drop
        - [ ] T-017 — Implement ordered wall kicks
            Scope: src/game/geometry.ts, pieces.ts; tests/game/geometry.test.ts.
            Depends on: T-016. Contracts: R01 in SPEC.
            Evidence: Both rotations try the specified offsets in order, honor hidden/edge/stack collisions, preserve O and reject fully blocked rotations.
        - [ ] T-018 — Implement shared regular landing
            Scope: src/game/landing.ts; tests/game/landing.test.ts.
            Depends on: T-017. Contracts: R06 in SPEC.
            Evidence: Deepest consecutively reachable legal landing handles edges/overhangs/zero distance and never mutates board, source or timers.
        - [ ] T-019 — Integrate hard drop with interval locking
            Scope: src/game/session.ts, timing.ts, src/app/input.ts; tests/game/ and tests/e2e/.
            Depends on: T-018. Contracts: R04/R06 in SPEC.
            Evidence: Space drops to shared landing, scores distance once, starts/preserves a captured lock timer and cannot reset it through repeated zero-distance input.
            Milestone exit: whole 2.2 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/2/2.2.md.
    - [ ] Milestone 2.3 — Modern play verification
        - [ ] T-020 — Verify modern keyboard and rendering flows
            Scope: tests/e2e/, tests/fixtures/, affected UI/controller fixes.
            Depends on: T-019. Contracts: R01/R04–R06/R12–R13 in SPEC.
            Evidence: Both browsers exercise bag/hold/preview/kick/drop paths with actual keyboard and visible evidence; regress classic lifecycle and board invariants.
        - [ ] T-021 — Prepare the modern-play distribution
            Scope: README.md, RELEASE-NOTES.md, package metadata, packaging/CI checks.
            Depends on: T-020. Contracts: R13 in SPEC.
            Evidence: Document new controls and fixed enabled capabilities, prepare v0.2.0 package/complete notes, inspect contents and pass relevant type/unit/build/browser regressions.
            Milestone exit: whole 2.3 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/2/2.3.md.
    - [ ] Milestone 2.4 — Phase review
        - [ ] T-022 — Review, test and report phase 2
            Depends on: T-016, T-019, T-021 and completion/verified hosted closure of delivery milestones 2.1–2.3.
            Scope: cross-milestone code review, phase exits, relevant prior regressions, build/types/unit and required Chromium/Firefox checks; repair blockers and verify prepared version/package/complete notes.
            Evidence: phase report and HANDOFF identify actual checks/limits and integration/release readiness; no claim of future merge/tag/publication. Report: docs/dev/reports/phases/2/PHASE-REPORT.md.

## Phase 3 — Configurable play aids

- [ ] Phase 3 — Configurable play aids
    - [ ] Milestone 3.1 — Ghost visibility
        - [ ] T-023 — Derive ghost visibility from shared landing
            Scope: src/game/landing.ts; tests/game/landing.test.ts.
            Depends on: T-022. Contracts: R06 in SPEC.
            Evidence: Hidden/dynamic/shown use min occupied y; straddling and boundary min-y 9/10 cases pass without consuming game state.
        - [ ] T-024 — Render the projected ghost
            Scope: src/ui/render.ts, styles.css; tests/ui/ and tests/e2e/.
            Depends on: T-023. Contracts: R06/R12 in SPEC.
            Evidence: Distinct outline projection matches occupied landing cells and remains readable against active/settled pieces after resize.
        - [ ] T-025 — Verify ghost and drop agreement
            Scope: tests/game/landing.test.ts, tests/e2e/, tests/fixtures/.
            Depends on: T-024. Contracts: R06/R13 in SPEC.
            Evidence: Upper/straddling/lower, overhang/edge, zero-distance and repeated redraw scenarios compare ghost to actual prelock hard-drop landing in both browsers.
            Milestone exit: whole 3.1 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/3/3.1.md.
    - [ ] Milestone 3.2 — Session settings and lock policies
        - [ ] T-026 — Define six typed session settings and controls
            Scope: src/settings/schema.ts, src/ui/controls.ts; tests/settings/.
            Depends on: T-025. Contracts: R09 in SPEC.
            Evidence: Canonical values/defaults and native labeled controls cover all six fields; invalid internal updates are rejected without corrupting settings.
        - [ ] T-027 — Implement alternative locking policies
            Scope: src/game/timing.ts, session.ts; tests/game/timing.test.ts.
            Depends on: T-026. Contracts: R04 in SPEC.
            Evidence: Failed gravity/soft descent and terminal hard-drop failure lock under failed-down; immediate hard drop locks under either policy; full-interval equality remains intact.
        - [ ] T-028 — Apply live setting changes coherently
            Scope: src/game/session.ts, src/app/controller.ts; tests/app/ and tests/game/.
            Depends on: T-027. Contracts: R04/R09 in SPEC.
            Evidence: General-policy changes restart/cancel timing as specified; hard-drop policy affects future drops; hold/preview toggles preserve source/slot and disabled commands are no-ops.
        - [ ] T-029 — Integrate settings with the running view
            Scope: src/ui/controls.ts, render.ts, src/app/input.ts; tests/e2e/.
            Depends on: T-028. Contracts: R09/R12 in SPEC.
            Evidence: Focused native controls cannot move pieces, all views reflect effective values, paused edits do not resume, and session defaults reapply only on new page load.
        - [ ] T-030 — Review, test and report milestone 3.2
            Depends on: T-026, T-027, T-028, T-029.
            Scope: whole 3.2 implementation and relevant dependencies, required focused tests/regressions and blocker repairs.
            Evidence: milestone exits satisfied; report with finding/TODO dispositions and HANDOFF committed with this task completion. Report: docs/dev/reports/phases/3/3.2.md.
    - [ ] Milestone 3.3 — Configurable experience verification
        - [ ] T-031 — Verify the session configuration matrix
            Scope: tests/e2e/, tests/settings/, tests/game/timing.test.ts.
            Depends on: T-030. Contracts: R04/R06/R09/R12–R13 in SPEC.
            Evidence: Both browsers exercise six controls, lock/drop combinations, ghost boundaries, pause/focus/resize and hidden preview/hold state preservation.
        - [ ] T-032 — Prepare the configurable-play distribution
            Scope: README.md, RELEASE-NOTES.md, package metadata and packaging checks.
            Depends on: T-031. Contracts: R13 in SPEC.
            Evidence: Document defaults/live rules and truthful session-only reload limitation; prepare v0.3.0 notes/package and pass prior-feature regressions.
            Milestone exit: whole 3.3 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/3/3.3.md.
    - [ ] Milestone 3.4 — Phase review
        - [ ] T-033 — Review, test and report phase 3
            Depends on: T-025, T-030, T-032 and completion/verified hosted closure of delivery milestones 3.1–3.3.
            Scope: cross-milestone code review, phase exits, relevant prior regressions, build/types/unit and required Chromium/Firefox checks; repair blockers and verify prepared version/package/complete notes.
            Evidence: phase report and HANDOFF identify actual checks/limits and integration/release readiness; no claim of future merge/tag/publication. Report: docs/dev/reports/phases/3/PHASE-REPORT.md.

## Phase 4 — Penetrator game

- [ ] Phase 4 — Penetrator game
    - [ ] Milestone 4.1 — Bonus activation
        - [ ] T-034 — Award and cap penetrator bonuses
            Scope: src/game/session.ts, types.ts; tests/game/session.test.ts.
            Depends on: T-033. Contracts: R08 in SPEC.
            Evidence: Only four-line clear earns one, capped at three; new sessions reset balance and updates occur before next spawn.
        - [ ] T-035 — Implement one-way activation and hold prohibition
            Scope: src/game/session.ts, src/app/input.ts; tests/game/session.test.ts.
            Depends on: T-034. Contracts: R07 in SPEC.
            Evidence: B spends exactly one with running eligible active piece; zero balance/repeated activation/hold are rejected; lock/restart never transfer or refund ability.
        - [ ] T-036 — Expose penetrator and bonus state
            Scope: src/ui/render.ts, controls.ts; tests/ui/ and tests/e2e/.
            Depends on: T-035. Contracts: R08/R12 in SPEC.
            Evidence: Activated cells are entirely opaque white; regular cells remain nonwhite, exactly one symbol per available bonus updates immediately and hold availability is accurate.
            Milestone exit: whole 4.1 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/4/4.1.md.
    - [ ] Milestone 4.2 — Traversal and legal landing
        - [ ] T-037 — Implement bounded penetrator traversal
            Scope: src/game/geometry.ts, session.ts; tests/game/geometry.test.ts.
            Depends on: T-036. Contracts: R07 in SPEC.
            Evidence: Movement and specified kick rotation ignore settled collisions only for active penetrator, retain occupied-cell bounds and never alter settled cells.
        - [ ] T-038 — Implement deepest legal penetrator projection
            Scope: src/game/landing.ts, session.ts; tests/game/landing.test.ts.
            Depends on: T-037. Contracts: R06–R07 in SPEC.
            Evidence: Shared ghost/hard-drop query scans through overlaps to deepest nonoverlapping placement at/below current y; no landing produces absent ghost/no-op hard drop without upward movement.
        - [ ] T-039 — Integrate penetrator grounding and legal locking
            Scope: src/game/timing.ts, session.ts; tests/game/timing.test.ts.
            Depends on: T-038. Contracts: R04/R07 in SPEC.
            Evidence: Ground only at deepest legal current placement; overlapping traversal starts no timer; activation/motion cancel only on airborne transition; invalid bottom terminal ends game without writes.
        - [ ] T-040 — Verify integrated penetrator transition scenarios
            Scope: tests/fixtures/, tests/game/session.test.ts, landing.test.ts, timing.test.ts.
            Depends on: T-039. Contracts: R04–R08 in SPEC.
            Evidence: Cavities, bottom overlap, hidden rows, no landing, zero-distance drop, both policies and activation-on-ground sequences preserve cells/source/bonus/hold invariants.
        - [ ] T-041 — Review, test and report milestone 4.2
            Depends on: T-037, T-038, T-039, T-040.
            Scope: whole 4.2 implementation and relevant dependencies, required focused tests/regressions and blocker repairs.
            Evidence: milestone exits satisfied; report with finding/TODO dispositions and HANDOFF committed with this task completion. Report: docs/dev/reports/phases/4/4.2.md.
    - [ ] Milestone 4.3 — Penetrator experience verification
        - [ ] T-042 — Verify penetrator rendering and interaction matrix
            Scope: tests/e2e/, controlled test fixtures and affected UI fixes.
            Depends on: T-041. Contracts: R04–R09/R12–R13 in SPEC.
            Evidence: Both browsers prove actual B/hold/drop/settings behavior, white cells/ghost/glyph rendering and normal play after penetrator lock; no test hook ships as production functionality.
        - [ ] T-043 — Prepare the penetrator distribution
            Scope: README.md, RELEASE-NOTES.md, package metadata and archive checks.
            Depends on: T-042. Contracts: R13 in SPEC.
            Evidence: Document bonus activation/traversal/hold/locking rules, prepare v0.4.0 notes/package and pass prior gameplay/settings regressions.
            Milestone exit: whole 4.3 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/4/4.3.md.
    - [ ] Milestone 4.4 — Phase review
        - [ ] T-044 — Review, test and report phase 4
            Depends on: T-036, T-041, T-043 and completion/verified hosted closure of delivery milestones 4.1–4.3.
            Scope: cross-milestone code review, phase exits, relevant prior regressions, build/types/unit and required Chromium/Firefox checks; repair blockers and verify prepared version/package/complete notes.
            Evidence: phase report and HANDOFF identify actual checks/limits and integration/release readiness; no claim of future merge/tag/publication. Report: docs/dev/reports/phases/4/PHASE-REPORT.md.

## Phase 5 — Persistent complete game

- [ ] Phase 5 — Persistent complete game
    - [ ] Milestone 5.1 — Settings restoration
        - [ ] T-045 — Implement canonical query and cookie codecs
            Scope: src/settings/codec.ts, schema.ts; tests/settings/codec.test.ts.
            Depends on: T-044. Contracts: R09–R10 in SPEC.
            Evidence: All values, malformed encodings, case/empty/unknown inputs and first duplicate URL/cookie occurrence are validated independently.
        - [ ] T-046 — Resolve independent startup precedence
            Scope: src/settings/codec.ts, src/app/controller.ts; tests/settings/restoration.test.ts.
            Depends on: T-045. Contracts: R10 in SPEC.
            Evidence: Per-field valid URL then valid cookie then default, mixed sources, omitted fields and invalid-first duplicate URL values produce the specified effective snapshot before session creation.
        - [ ] T-047 — Plan and apply explicit URL cookie synchronization
            Scope: src/settings/persistence.ts, controller.ts; tests/settings/restoration.test.ts.
            Depends on: T-046. Contracts: R10–R11 in SPEC.
            Evidence: Through injected cookie ports, startup saves each valid explicit URL including defaults and never overwrites cookies for absent/invalid parameters; adapter invocation order is observable.
            Milestone exit: whole 5.1 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/5/5.1.md.
    - [ ] Milestone 5.2 — Persistence adapters
        - [ ] T-048 — Implement scoped cookie writes and failure detection
            Scope: src/settings/persistence.ts; tests/settings/persistence.test.ts.
            Depends on: T-047. Contracts: R11 in SPEC.
            Evidence: Encoded field cookies use required Path/Max-Age/SameSite/Secure attributes, preserve unrelated cookies and report exception or failed readback independently.
        - [ ] T-049 — Implement URL replacement without navigation
            Scope: src/settings/persistence.ts; tests/settings/persistence.test.ts.
            Depends on: T-048. Contracts: R11 in SPEC.
            Evidence: Only changed key is canonicalized once, unrelated query/path/hash survive, replaceState adds no history entry and failure is isolated from cookie writes.
        - [ ] T-050 — Integrate persistent settings and warnings
            Scope: src/app/controller.ts, src/ui/controls.ts, src/settings/persistence.ts; tests/e2e/.
            Depends on: T-049. Contracts: R09–R12 in SPEC.
            Evidence: Startup/change/reload flows, explicit defaults, omitted fields and independently denied adapters retain playable in-memory values and destination-specific warning recovery in both browsers.
            Milestone exit: whole 5.2 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/5/5.2.md.
    - [ ] Milestone 5.3 — Complete product verification
        - [ ] T-051 — Verify the complete clean-checkout product
            Scope: tests/e2e/, tests/fixtures/, CI and documented setup.
            Depends on: T-050. Contracts: R01–R13 in SPEC.
            Evidence: Clean install/type/unit/build and both isolated browsers cover final contract matrix, representative actual play/rendering/reload, repeated contexts/teardown and all delivered regressions; record precise versions and limits.
        - [ ] T-052 — Prepare the complete static release
            Scope: README.md, RELEASE-NOTES.md, package metadata and scripts/package.mjs.
            Depends on: T-051. Contracts: R13 in SPEC.
            Evidence: Prepare v1.0.0 exact complete release message, inspect archive/manifest/checksums and exclude secrets/debug fixtures/binaries; validate user setup/play/configuration documentation and static artifact reproducibility.
            Milestone exit: whole 5.3 code review, required checks/repairs, committed report and HANDOFF; depends on every earlier task in this milestone through the chain above.
            Report: docs/dev/reports/phases/5/5.3.md.
    - [ ] Milestone 5.4 — Phase review
        - [ ] T-053 — Review, test and report phase 5
            Depends on: T-047, T-050, T-052 and completion/verified hosted closure of delivery milestones 5.1–5.3.
            Scope: cross-milestone code review, phase exits, relevant prior regressions, build/types/unit and required Chromium/Firefox checks; repair blockers and verify prepared version/package/complete notes.
            Evidence: phase report and HANDOFF identify actual checks/limits and integration/release readiness; no claim of future merge/tag/publication. Report: docs/dev/reports/phases/5/PHASE-REPORT.md.
            Final outcome: also commit docs/dev/reports/IMPLEMENTATION-REPORT.md aggregating all milestone/phase unresolved or deferred TODOs, options, owners, resolutions and evidence. Full-project completion requires the verified phase integration and release.
