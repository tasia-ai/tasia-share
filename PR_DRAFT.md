# feat(ui): detachable native windows for multi-monitor layouts

No issue

## What it is for

Users with multiple displays can move selected UI floaters, including Conversations, out of the main viewer and position them on a second or third monitor. The main 3D scene remains in the original viewer window, without an additional login or renderer.

## Legacy Viewer origin

The expected user-facing workflow follows the Legacy Viewer's detached utility windows and standard desktop multi-monitor behaviour. The underlying implementation is native Rust/winit/egui, not a port of the Legacy Viewer's C++ window manager.

## What changes

- **UI:** optional native tear-off and reattachment for existing floaters; stable native viewport identity for multiple conversations, preserving current visual styling
- **Window handling:** independent native-window event routing, painter lifecycle, and shared egui texture updates, without a second 3D renderer
- **Displays:** persistent monitor-aware placement, scale/DPI handling and recovery if a previously used display disappears
- **Preferences:** window display recovery enabled by default
- **Localization:** strings for the detachable-window controls and recovery setting in English, French and Japanese
- **Dependencies:** enable egui-wgpu winit integration and declare pollster for native viewport setup
- **Compatibility:** leave existing fullscreen behaviour, chat input, scene rendering and default in-viewer layout unchanged

## Screenshots

Not provided yet. The source code has been used in a downstream viewer over an extended period, but this exact rebased upstream snapshot has not been visually recorded.

## Tests

- [x] Linux `cargo check --locked -p aurora-viewer` against Aurora main `069f41e`, [successful CI](https://github.com/tasia-ai/tasia-share/actions/runs/42518001909) (before the Japanese translation addition)
- [x] `git apply --check`, `git diff --check` and Rustfmt on the same reviewed revision
- [x] Scan of contributed patch/code for unrelated branding
- [ ] Re-run CI with Japanese localization changes and latest Aurora main
- [ ] Complete unit tests, Clippy, and project CI checks
- [ ] Windows/macOS compilation
- [ ] Record human multi-monitor validation against a build with this **exact upstream rebased diff**

**User feedback:** the underlying multi-window implementation has been in practical use in a downstream viewer for an extended period and is considered validated there. This does not establish that this particular Aurora integration was executed on multi-monitor hardware.

## Limits and follow-up

- The patch intentionally excludes experimental process-per-floater isolation, external service integrations, branding, and unrelated changes.
- The implementation contains a scoped unsafe event-loop pointer; it requires an explicit lifetime/safety review.
- Confirm GPU/device-loss recovery and detached-window texture cleanup on Windows/Linux/macOS.
- The current `tasia-share` repository is a patch distribution repository, **not** a GitHub fork of Aurora, so this draft must be submitted from a real fork branch containing the source changes rather than the patch file.

## Contributor note

Proposed implementation: [standalone patch](https://github.com/tasia-ai/tasia-share/blob/main/0003-aurora-069f41e-rebased.patch). All changes must be rebased onto the actual upstream PR base and verified before submission.
