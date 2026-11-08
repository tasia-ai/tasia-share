# Porting notes — not yet PR-ready

## Baseline

Development source: multi-screen implementation added in October 2026, with later refinements. The reference patch preserves selected implementation hunks rather than all changes from that development commit.

Target: `Aurora-Viewer/Aurora-Viewer` at a **fresh current main** checkout.

**Unverified:** No upstream branch was created, no compilation was run, and no native multi-monitor interactive test was run while preparing this package. The current upstream `app.rs` is too large for a reliable full-file read through the available interface.

## Required adaptation

- `app.rs`: integrate the window-host initialization, event dispatch, redraw loop, texture deltas and shutdown against current upstream. Validate the geometry persistence and monitor-recovery paths.
- `ui/widgets.rs`: rebase all Floater changes, especially embedded/native state transfer, title-bar drag and reattachment, without changing existing window semantics.
- `ui/chat/popout.rs`: add stable per-conversation native viewport identity tests and make sure each chat panel reattaches correctly.
- `main.rs`: register only the native viewport and window display modules. **Do not** register or launch the experimental process-isolation prototype.
- `settings.rs`, `settings/backup.rs`, Layout preferences: integrate new fields without data loss or breaking settings migrations.
- Locales: the English and French strings are in the reference patch; update Japanese and other localizations to match current locale structure.
- Cargo: enable the required egui-wgpu winit integration for the exact version pinned in upstream and update lockfile with Cargo.
- Validate closing secondary windows, preserving/releasing textures, mouse capture, drag-and-drop, clipboard and IME input.

## Branding and scope gate

Before any upstream submission, inspect **the full diff against Aurora upstream**, not just the changed files. Reject the PR if it includes viewer branding, unrelated source files, custom services, private endpoints, tokens, secrets, or a changed README, release configuration or assets.

The source files in this share repository are normalized, but that fact does **not** guarantee the source repository's baseline is suitable for a pull request.

## Safety review

The native viewport host uses an unsafe dereference of an event-loop pointer managed within a synchronous scope with a Drop guard. Review that the pointer never escapes the scope and that callbacks cannot invoke it after teardown. Confirm correct child-window event routing and no UI or texture resources remain after close.

Treat a successful unit test as necessary, not sufficient; verify with actual two- and three-monitor setups and DPI changes.
