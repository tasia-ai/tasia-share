# Aurora Viewer — native multi-monitor UI contribution

A standalone, branding-free contribution package for [Aurora Viewer](https://github.com/Aurora-Viewer/Aurora-Viewer).

**Status:** rebased and compile-checked against Aurora main commit `069f41e84c3e64a0a636b95062b47dcf552c3efc` (8 November 2026). This repository is **not** an Aurora fork and no upstream PR has been opened.

## Recommended patch

**[`0003-aurora-069f41e-rebased.patch`](0003-aurora-069f41e-rebased.patch)** is the complete patch for the pinned Aurora revision. It combines the native viewport host, monitor/DPI recovery, floater tear-off support, settings, UI strings, Cargo dependency and lockfile changes, and the conversation viewport identity regression test.

The older `0001-...` and `0002-...` patches remain **historical development references only**. Do not apply them on current Aurora main or on top of `0003`.

The companion Rust modules are in [`src/`](src/). They are included in `0003`, so do **not** copy them a second time when applying the complete patch.

## Verified automated checks

[GitHub Actions successful compile run](https://github.com/tasia-ai/tasia-share/actions/runs/42518001909)

- [x] `git apply --check` and `git apply` against a clean Aurora checkout at the pinned commit
- [x] `git diff --check`
- [x] Rustfmt syntax/format check on modified Rust files, using Aurora's `rustfmt.toml`
- [x] Contribution branding scan
- [x] Linux `cargo check --locked -p aurora-viewer`

**Not yet verified:** full unit test suite, Windows/macOS compilation, and physical multi-monitor interaction (2–3 screens, cross-monitor DPI changes, reattach/focus and disconnection recovery). These remain prerequisites for an upstream-ready PR.

## What it adds

- Native UI-only egui viewports for detached floaters, with a shared viewer state and no duplicate 3D scene renderer
- Tear-off and reattachment of selected UI panels, including conversation windows
- Monitor-aware native-window placement with DPI correction and off-screen recovery
- Independent keyboard/mouse event routing for detached windows
- Tests for stable viewport identities and display placement

## How to apply to the reviewed Aurora revision

```bash
git clone https://github.com/Aurora-Viewer/Aurora-Viewer.git
git clone https://github.com/tasia-ai/tasia-share.git
cd Aurora-Viewer
git checkout 069f41e84c3e64a0a636b95062b47dcf552c3efc
git apply --check ../tasia-share/0003-aurora-069f41e-rebased.patch
git apply ../tasia-share/0003-aurora-069f41e-rebased.patch
cargo fmt --all -- --check
cargo check --locked -p aurora-viewer
```

Linux build dependencies may need to be installed as in the workflow. For a newer Aurora commit, rebase and rerun all checks.

## Files and scope

- [`TEST_PLAN.md`](TEST_PLAN.md) — functional, platform and regression checklist
- [`PORTING_NOTES.md`](PORTING_NOTES.md) — integration boundaries and remaining work
- [`ISSUE_DRAFT.md`](ISSUE_DRAFT.md) — proposed upstream feature discussion
- [`.github/workflows/aurora-port-check.yml`](.github/workflows/aurora-port-check.yml) — reproducible hosted validation

Excluded intentionally: viewer branding, logos, skins, startup changes, private configuration, custom services, unrelated renderer/network changes, and the experimental process-per-floater prototype.

The code retains the applicable GPL-3.0-or-later licensing and existing authorship requirements. The native viewport host's scoped unsafe event-loop pointer needs an explicit safety review before upstream submission.
