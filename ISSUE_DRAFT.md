# Feature proposal: native detachable viewer panels across multiple monitors

Would the Aurora maintainers be interested in an optional native multi-window feature for selected floaters, particularly Conversations?

The idea is to let users drag a panel outside the main viewer and place it on a second or third physical display. The 3D world would continue rendering only in the main viewer; extra windows would render UI through native egui viewports and share the existing viewer state.

Proposed scope:
- Native tear-off and reattachment for selected floaters.
- Independent mouse/keyboard input and focus in detached windows.
- DPI-aware monitor placement and safe recovery when a monitor is disconnected.
- Remembered window geometry without trapping windows off-screen.
- No additional login, world session or secondary 3D renderer.

An early Rust prototype and unit tests exist. It has **not** yet been fully validated against current Aurora main or across Windows/Linux multi-monitor configurations. The intention is to submit an isolated patch after rebasing and testing, not an unrelated fork customization.

If this direction fits Aurora's roadmap, would maintainers prefer a first PR limited to display placement/recovery, followed by native detachable floaters as a separate PR?
