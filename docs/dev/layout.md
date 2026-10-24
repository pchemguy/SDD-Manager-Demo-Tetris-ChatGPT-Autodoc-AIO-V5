# Repository layout

## Status and ownership

The repository currently contains policies, disclosure/provenance and development documents. Paths below allocate the intended implementation; their listing does not mean source files or commands already exist. Logical owners are defined in [DECOMPOSITION](DECOMPOSITION.md), behavior in [SPEC](SPEC.md), and increments in [PLAN](PLAN.md).

| Intended path | Owner and contents | Checks / collaboration |
| --- | --- | --- |
| `src/game/types.ts` | Shared serializable board/piece/session/action/settings-facing types; no browser imports | Used by core and controller; avoid a miscellaneous utilities owner |
| `src/game/geometry.ts`, `pieces.ts` | C2 grid/shape/orientation/placement/kick rules | `tests/game/geometry.test.ts`, shape/edge fixtures |
| `src/game/source.ts` | C3 random sequence and seven-bag state | `tests/game/source.test.ts`; explicit seeded fixtures |
| `src/game/landing.ts` | C4 regular and penetrator projection / visibility | `tests/game/landing.test.ts`; tunnel/midline/no-landing fixtures |
| `src/game/timing.ts`, `session.ts` | C5 simulated timing helpers and C1 authoritative transitions/clear/score/hold/bonus lifecycle | `tests/game/timing.test.ts`, `session.test.ts`; integrated transition sequences |
| `src/app/controller.ts`, `input.ts`, `clock.ts` | C6 session ownership and browser event/time lifecycle | `tests/app/`; fake ports, focus/repeat/teardown seams |
| `src/settings/schema.ts`, `codec.ts`, `persistence.ts` | C7 typed defaults/validation and URL/cookie adapters | `tests/settings/`; per-field precedence/codec/failure matrices |
| `src/ui/render.ts`, `controls.ts`, `styles.css` | C8 Canvas/read-only views and native controls | `tests/ui/` where useful, plus `tests/e2e/` rendering/input evidence |
| `src/main.ts`, `index.html` | Composition root and static document; one controller/loop/listener lifecycle | Actual build/serve path in end-to-end checks |
| `tests/fixtures/` | Reusable explicit board/seed/event fixtures owned by their consuming tests | Test-only import graph; never bundled by application entry |
| `tests/e2e/` | Playwright Test application flows and Canvas/white/glyph/ghost evidence | Chromium primary and managed Firefox secondary; independent isolated contexts |
| `scripts/browser-env.mjs` | Project-owned absolute Playwright binary path and finite install/test process entry | Same path used by provisioning and every runner process; direct Node execution on Windows |
| `scripts/package.mjs` | Static release archive and manifest/checksum preparation | Inspect archive contents and version correspondence; no credentials/test binaries |
| `package.json`, `package-lock.json` | Pinned compatible runtime/build/test toolchain and declared scripts | Clean install/type/unit/build checks; exact versions selected and recorded during implementation |
| `tsconfig.json`, `vite.config.ts`, `vitest.config.ts`, `playwright.config.ts` | Type/build/unit and isolated two-browser application runner configuration | Keep runtime code independent of build/test configuration |
| `.github/workflows/ci.yml` | Clean checkout, pinned install and relevant type/unit/build/browser checks | Same declared commands and dedicated binary path; no PAT embedded |
| `README.md` | Supported setup/play/check instructions and honest delivered scope | Commands become authoritative after implementation validates them |
| `RELEASE-NOTES.md` | Complete current release message/version scope | Final message is checked before release commit/tag/GitHub publication |
| `docs/dev/` | Current design, specification, delivery, executable tasks and adjacent preparation review reports | DOCS_TASKS and DOCS_PREPARATION remain separate owners |
| `docs/dev/reports/phases/<phase>/` | Milestone `<milestone>.md` and `PHASE-REPORT.md` evidence | Task-owned reports/HANDOFF updates; reports document actual checks and limits |
| `docs/dev/reports/IMPLEMENTATION-REPORT.md` | Final complete implementation summary and aggregated TODO disposition | Final phase review owns it; no premature completion claim |
| `docs/ai-logs/` | Existing manually maintained provenance scaffold/logs | Nested AGENTS applies; excluded from routine source/contract validation |

## Dependency and allocation rules

Core imports only other core modules and explicitly passed validated settings; shared types must not import app/UI/storage. The controller composes core and settings; UI receives snapshots and commands. Tests may import public/core seams; production imports cannot reach tests. Keep helpers with the owning component until multiple real consumers justify another boundary. Geometry and landing are separate because penetrator/ghost policy changes differ from static shape rules.

Keep pure configuration decoding separate from cookie/history operations within C7. A task can change a few collaborating modules where a contract crosses boundaries; no speculative wrapper layer is required. Source/test placement permits the five capability increments without relocating an earlier implementation or introducing a second executable task list.

## Generated and sensitive paths

`dist/` holds the static product, `artifacts/` release packages, `.cache/playwright/` dedicated managed browser binaries, and `test-results/`, `playwright-report/`, `coverage/` and temporary owned setup files hold verification output. These paths and `node_modules/` must be ignored when introduced; keep them outside production entry/import paths. Resolve the managed browser location to an absolute path for all install/run processes. Do not use a personal/system installation or default user cache as an implicit substitute.

The existing ignored conventional repository token remains outside tracked source and product packages. Preserve existing ignore rules. Do not modify `/pyenv` or global browser/tool installations. On Windows use CMD/direct Node and follow WINDOWS_SHELL_POLICY.md; Linux uses its native shell. A launcher must not implicitly invoke PowerShell.

## Planned command ownership

Implementation will establish scripts for clean dependency installation, type checks, unit checks, build, dedicated browser provisioning, end-to-end checks and static packaging. Exact invocations and compatible versions belong in package metadata and README after validation. This layout does not claim uncreated scripts are runnable.

Current root AGENTS navigation links the established documents and coordinator recovery. Add TASKS navigation with its creation; add validated setup/check commands only when implementation provides evidence. No additional nested instructions are needed for ordinary source folders.
