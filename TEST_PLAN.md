# Validation plan for native detachable windows

## Code / build
- [x] Patch applies to Aurora `069f41e` with `git apply --check` and `git diff --check`.
- [x] Rustfmt syntax/format check of modified files using Aurora's `rustfmt.toml`.
- [x] Linux `cargo check --locked -p aurora-viewer` ([successful run](https://github.com/tasia-ai/tasia-share/actions/runs/42518001909)).
- [x] Static scan of contributed code/patch for unrelated project branding.
- [ ] Run full-workspace `cargo fmt --all -- --check`, Clippy and required project CI on a clean upstream branch.
- [ ] Run the `window_display` and `native_viewports` unit tests, plus all affected `widgets` and `chat/popout` tests.
- [ ] Confirm original window placement continues to work with this feature disabled.
- [ ] Audit `unsafe` event-loop pointer usage, native event lifetime and texture-delta order.

## Interactive behavior
- [ ] Open two or more independent IM conversations; detach both into separate native windows.
- [ ] Move one to monitor 2 and one to monitor 3; type, select text, scroll and resize both.
- [ ] Dock each panel back into the main viewer; close a detached window; reopen.
- [ ] Ensure focus on a detached panel does not incorrectly trigger the main window's background-FPS mode.
- [ ] Verify root window keeps rendering the 3D world without an additional scene renderer or second login.
- [ ] Verify clipboard, IME, emoji input, keyboard shortcuts, popup menus and drag-to-move behavior.

## Displays and recovery
- [ ] Mix 100%, 125%, 150% or 200% DPI across monitors.
- [ ] Change primary monitor, monitor order and screen origin (including negative coordinates).
- [ ] Disconnect a monitor while windows are on it; recover reachable title bars.
- [ ] Restart with one previously used monitor disconnected; restore sensible placement.
- [ ] Repeat maximized, minimized and fullscreen transitions without saving invalid geometry.
- [ ] Test 1/2/3-monitor layouts on Windows and Linux; macOS if maintainers support it.

## Regression and performance
- [ ] No blank 3D frame, crashes, leaked detached windows, lost texture updates or stuck mouse capture.
- [ ] No extra world session and no unnecessary duplicate scene renderer.
- [ ] Check GPU usage, FPS, memory and input responsiveness with 0, 1 and 3 detached floaters.
- [ ] Confirm translations, accessibility/focus behavior, layout migration and existing settings work.

Record actual pass/fail evidence in the eventual upstream PR. Checked automated gates above are verified; all unchecked tests still need to be performed.
