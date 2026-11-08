# feat(ui): native detachable windows for multi-monitor layouts

No issue

## What it is for

Users with multiple displays can move supported UI floaters, including Conversations, into independent native desktop windows. The main 3D scene stays in the primary viewer, without an extra login or duplicate scene renderer.

## Legacy Viewer origin

The expected behavior follows separate utility windows in the Legacy Viewer, implemented here natively in Rust/egui/winit rather than copying its C++ window management.

## What changes

- **Interface:** detachable/reattachable floaters with independent window input, focus and stable per-conversation viewport IDs.
- **Rendering:** UI-only painter for additional native windows, using the existing egui context and shared texture updates.
- **Window placement:** monitor-aware geometry recovery, mixed-DPI support and safe fallback for disconnected monitors.
- **Preferences:** display recovery option enabled by default.
- **Localization:** new UI strings in English, French and Japanese.
- **Dependencies:** native egui-wgpu window support, pollster and corresponding Cargo lockfile changes.
- **Scope:** no unrelated branding, themes, service integrations or renderer/network modifications.

## Screenshots

No screenshots from the exact rebased upstream version are available yet. The corresponding functionality has been used in a downstream viewer over an extended period.

## Tests

- [x] [Patch application, formatting, branding checks and Linux `cargo check --locked -p aurora-viewer` on Aurora `1efb1f9f`](https://github.com/tasia-ai/tasia-share/actions/runs/42520286821).
- [x] Single clean commit in a real Aurora fork, affecting exactly 14 implementation, configuration and translation files.
- [ ] Aurora's PR CI, full workspace unit tests, Clippy and Windows/macOS platform checks.
- [ ] Human validation of multi-monitor interactions against this exact upstream rebased build.

**Human feedback:** the underlying feature has been used regularly in a downstream viewer for a long period. This is relevant functional experience, but it does not establish direct testing of this exact upstream build.

## Limits and follow-up

- This should be treated as a **draft PR** until the outstanding CI and platform checks and the final human interaction verification have completed.
- Review the scoped unsafe event-loop pointer's lifetime and teardown, native window lifecycle, DPI change handling and secondary texture cleanup.
- The experimental child-process floater prototype is not included.
- Related issues: #277 (detachable import window) and #1090 (multiple inventory windows); this PR supplies the native-window foundation, not independent duplicate inventories.

## Submission

Proposed source branch: [`tasia-ai:feat/native-multimonitor-floaters`](https://github.com/tasia-ai/Aurora-Viewer/tree/feat/native-multimonitor-floaters).

Target: `Aurora-Viewer/Aurora-Viewer:main`. All actual changes live in this fork branch, not in a standalone patch file. Please open as **Draft pull request**.
