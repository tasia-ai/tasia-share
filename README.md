# Aurora Viewer: native multi-monitor UI contribution (staging)

This repository is an **isolated, branding-free upstream porting package**. It is not an upstream fork, a ready-to-merge pull request, or a verified build against current Aurora main.

## Goal

Allow selected in-viewer floaters (especially Conversations) to detach into native, movable desktop windows on other monitors while the 3D world remains rendered only in the main viewer window.

## Included

- `src/native_viewports.rs` — egui immediate-viewports host; native window lifecycle, input, texture exchange and unit tests.
- `src/window_display.rs` — monitor matching, DPI-aware position restore, off-screen recovery and unit tests.
- `0001-multiscreen-porting-reference.patch` — selected code changes from a development prototype, with unrelated process-isolation code removed and viewer-specific identifiers normalized.
- `0002-chat-viewport-identity-test.patch` — regression test for unique, stable detached conversation viewport IDs.
- `TEST_PLAN.md` — functional, platform, safety and compatibility checks.
- `ISSUE_DRAFT.md` — proposed upstream discussion text.
- `PORTING_NOTES.md` — rebase/merge blockers and engineering guidance.

## Explicitly excluded

- Branding, logos, URLs, release channels, skins, theme overrides and startup changes.
- Private configuration, service integrations and deployment settings.
- The separate-process detached-floater prototype (experimental; not required).
- Unrelated renderer, networking and appearance changes.

## Upstream preparation

1. Create an actual **public fork of Aurora-Viewer/Aurora-Viewer** through GitHub; this share repository is **not** an Aurora fork and cannot be used directly as a pull request head.
2. In a fresh branch from **current upstream main**, selectively port the modules and patch. First try `git apply --check 0001-multiscreen-porting-reference.patch`; this reference diff was created against an earlier codebase and may need manual rebasing.
3. Reconcile `app.rs`, `ui/widgets.rs`, `settings.rs`, and the localization files with the current architecture. The current upstream `app.rs` was not fully retrievable for a verified rebase.
4. Ensure each detached conversation has a stable native viewport ID. Port the associated chat popout identity test if missing.
5. Run formatting, linting, unit tests, live interactive monitor tests and project CI; capture results in the PR.
6. Do not propose this staging branch itself as an upstream PR.

## Notes

The code is intended to preserve Aurora's existing user experience unless a floater is explicitly detached. No additional 3D scene renderer, world session or login is created. The native viewport host includes a scoped unsafe event-loop pointer: this requires an explicit safety review when porting.

Source and derivatives remain subject to the applicable GPLv3 and existing copyright/attribution notices; do not strip authorship from original files.
