# Aurora multi-monitor port — technical notes

## Reviewed baseline and build result

- Aurora upstream: `069f41e84c3e64a0a636b95062b47dcf552c3efc` (8 November 2026).
- Complete rebased patch: `0003-aurora-069f41e-rebased.patch`; supersedes historical reference patches `0001` and `0002`.
- [Successful hosted validation](https://github.com/tasia-ai/tasia-share/actions/runs/42518001909): patch applies, whitespace and Rustfmt checks pass, no unintended project branding found, `cargo check --locked -p aurora-viewer` succeeds on Linux.
- **Not yet validated:** full unit tests, Windows/macOS builds, actual interaction across multiple monitors and display hot-plug scenarios.

## Integration details

- `app.rs`: initializes/tears down the native viewport host; routes secondary-window events to it; shares egui texture updates and uses the event loop's synchronous scope for root UI passes. The port preserves current fullscreen handling and recent chat/input changes.
- `ui/widgets.rs`: bridges Floater embedded/native state, native drag, reattachment and root geometry restoration.
- `ui/chat/popout.rs`: tests stable unique IDs for multiple detached conversations.
- `native_viewports.rs`: native window/input/texture lifecycle and unit tests, with one shared egui Context and a separate UI painter (no extra 3D scene renderer).
- `window_display.rs`: display matching, placement restore, DPI changes, and fallback for unreachable/off-screen windows.
- `settings.rs`, `settings/backup.rs`, Layout preferences and translations: persist the display recovery option and display bounds without overwriting unrelated settings.
- `Cargo.toml` and `Cargo.lock`: enable `egui-wgpu` winit integration and declare `pollster` for native painter initialization.

## Next validation before proposing an upstream PR

1. Run targeted `cargo test -p aurora-viewer` tests covering `native_viewports`, `window_display`, chat popouts and affected UI logic; then broader project tests and Clippy.
2. Verify native tear-off, typing, focus, clipboard, drag, resize, docking/reattachment, close and monitor disconnect/reconnect on Windows and Linux hardware with mixed DPI.
3. Rebase again onto the precise Aurora main revision chosen for upstream review; repeat all checks.
4. Create an actual public GitHub fork of Aurora (this repository is not a fork), commit just the focused code changes, and open a draft PR after maintainers agree on the feature scope.

## Safety / scope

Review the scoped unsafe event-loop pointer and ensure it never outlives a synchronous viewport callback. Check texture cleanup, window close behavior and correct event routing. Compilation success is not runtime validation.

Never copy viewer branding, logos, themes, service endpoints, private credentials, release settings, unrelated changes, or the experimental process-per-floater prototype into the upstream PR. Audit the *full diff against Aurora upstream*.

Retain applicable GPL-3.0-or-later source license and original author notices.
