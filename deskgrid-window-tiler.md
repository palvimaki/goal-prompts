# DeskGrid: Tailored Local Window Tiling App

Copy the prompt below into a frontier-level coding agent. It is written to be
portable across operating systems, host environments, and implementation stacks.
For Codex, prefix the first instruction with `/goal` if you want Codex goal
mode. For other agents, paste it as-is.

````markdown
Your goal is to build a complete local-first window tiling app, called DeskGrid unless the user prefers another name, that gives the user one fast "make my desk usable" action.

You are a frontier-level coding agent working on the user's own computer. Build the actual app, not just a plan. Adapt the implementation to the user's operating system, installed tools, display setup, app habits, accessibility permissions, and tolerance for setup. Keep it small, local, reversible, and pleasant.

## Product Spirit

DeskGrid is not a dashboard, workspace manager, or power-user puzzle. It is a tiny desk-reset button. The best version feels like the computer noticed how the user already works and quietly made that repeatable.

One action arranges the current desk. One action saves the current desk. One action recalls saved desks. Everything stays local.

The app should have a little personality, but only after the core utility is reliable. Use short, calm, user-tailored copy. Do not add onboarding theater, marketing screens, gamification, or decorative complexity.

## First-Launch Rule

First launch must be genuinely first launch:

- Do not ship the author's saved layout.
- Do not hardcode the author's app names, paths, screenshots, displays, or preferences.
- Do not assume the user knows what DeskGrid is.
- Do not require an account, cloud service, or tutorial.
- Ask at most one necessary question.

On first launch, show a readable, non-editable prompt similar in spirit to:

> DeskGrid found the windows and displays currently in use. Arrange this desk now?

Offer concise actions appropriate to the OS and host UI:

- Smart arrange now.
- Save this desk exactly as it is.
- Not now.

Avoid editable search fields or chooser title fields for this first-launch surface. If a permission is required, explain the permission in plain language and ask only at the moment it is needed.

## Launching For Non-Technical Users

Launching must be obvious after install. Offer a one-click/native action that creates a simple launcher:

- macOS: Desktop `.command`, small `.app` wrapper, Automator-style app, or native packaged app shortcut.
- Windows: Desktop or Start Menu `.lnk`.
- Linux: `.desktop` launcher or desktop-environment-specific equivalent.

The launcher should only open the app or host process. It must not duplicate setup logic, hide errors, or require shell knowledge. Put "Create Desktop launch shortcut" or the OS-native equivalent in the tray/menu UI and mention it in the shortcut hint.

## Required UX

Build the smallest complete app with:

- menu bar, tray, panel, or extension entry point;
- Smart Arrange;
- Save Current Desk;
- Pick Saved Desk;
- Explain Inferred Desk;
- Show Shortcuts;
- Create Desktop Launch Shortcut;
- First Launch Simulation or Reset First Launch, for testing;
- Export Blueprint or Export Spec, containing no personal data;
- Reload or restart action if the host environment needs it.

Every time the app opens, show a small, unintrusive shortcut hint listing the core commands. It must be visible enough to help a new user and quiet enough not to become annoying.

Prefer hotkeys as the fastest path, but do not require the user to memorize them before using the app.

## Smart Arrange Behavior

Use local window metadata and display geometry first:

- screen count and usable frame sizes;
- current focused window;
- front-to-back window order;
- window app name, bundle/process/class, and title;
- current window size and screen;
- whether each window is visible, standard, movable, minimized, or fullscreen.

Screenshots are optional. They may help a user-specific agent reason about visual context, but they require extra permission and can capture private content. Do not use screenshots by default. Ask only if there is a clear UX benefit.

Classify windows into simple roles:

- Main: browser, editor, design canvas, document surface, primary creative/work surface.
- Side: assistant, notes, task tracker, project management, secondary work surface.
- Terminal: shell, logs, development consoles.
- Utility: files, chat, calendar, mail, meetings, previews, small helpers.

Rank windows by local importance signals:

- focused window first;
- front-to-back order;
- role priority: main, side/notes/tasks, terminal, utility;
- larger durable windows before tiny helper windows;
- recently visible and non-duplicate windows before duplicate clutter.

Before minimizing anything, use the whole screen:

- create only columns or zones that actually have windows;
- redistribute empty-zone width to active groups;
- avoid leaving blank screen real estate when useful windows remain;
- keep main app groups visible whenever possible.

If there are too many windows to fit cleanly, keep the best-fitting set visible and automatically minimize lower-priority overflow. First minimization candidates are duplicate utility windows, stray file-manager windows, previews, temporary text windows, leftover helper windows, and extra duplicate editor windows. Do not minimize the app's own control window or obvious primary work surfaces unless the screen is too small to keep them usable.

## Layout Strategy

Choose layout from display shape:

- Wide external display: terminal rail, side rail, large main column, utility rail.
- Ordinary desktop or laptop display: large main area plus stacked side rail.
- Small display: one main window first, then secondary windows stacked or left untouched.
- Multiple displays: arrange each display independently unless the user clearly prefers a single primary display workflow.

Saved layouts may restore exact windows and positions when the user explicitly saved them. Smart Arrange should not require a hardcoded list of apps. If app-specific rules are useful, infer them from the current user and store them as editable local preferences.

## Privacy And Storage

Keep all data local:

- no telemetry;
- no account;
- no cloud sync;
- no network dependency;
- no analytics;
- no saved screenshots by default;
- no secrets or private identifiers in exported specs.

Store simple local JSON, plist, registry, SQLite, or desktop-environment-native config, whichever is most normal for the chosen stack. Back up any existing config before overwriting it.

Suggested preference shape:

```json
{
  "version": 2,
  "firstRunComplete": true,
  "defaultLayout": "smart",
  "showMenuOrTray": true,
  "lastAppliedLayout": "smart"
}
```

Suggested saved layout shape:

```json
{
  "kind": "snapshot",
  "saved_at": "2026-01-01T12:00:00Z",
  "screen": { "w": 1440, "h": 900 },
  "windows": [
    {
      "app_id": "example.browser",
      "title": "Example Window",
      "frame": { "x": 0, "y": 0, "w": 900, "h": 700 }
    }
  ]
}
```

## Stack Selection

Choose the simplest native path that can actually control windows on the user's system.

macOS options:

- Hammerspoon Spoon for a hackable personal utility.
- Swift/SwiftUI plus Accessibility APIs for a packaged native app.
- Respect the menu bar and Dock with usable screen frames.
- Use Accessibility permission only for window control and explain it clearly.

Windows options:

- Native tray app using .NET, WinUI, WPF, Win32 APIs, or AutoHotkey if that is the user's practical environment.
- Respect DPI scaling, taskbar work areas, virtual desktops where feasible, and elevated-window limitations.
- Do not fight built-in Snap unless the user explicitly wants replacement behavior.

Linux options:

- Integrate with the user's actual environment: GNOME Shell extension, KDE/KWin script, Sway/Hyprland IPC, or X11/EWMH fallback.
- Respect panels, workspaces, compositor limitations, and Wayland permission boundaries.

If the ideal stack is unavailable, build the best working local version with the least setup friction and explain the tradeoff briefly.

## Implementation Rules

- Build the app in the user's environment.
- Keep code minimal and readable.
- Prefer platform-native APIs and proven window-management libraries over fragile hacks.
- Do not add speculative features beyond this prompt.
- Keep installation reversible.
- Avoid destructive actions.
- Back up existing configuration before modifying it.
- If permissions are required, request or guide them at the exact point of need.
- If the app cannot control windows on the current OS without user action, still build the app and make the blocker explicit.

## Verification

Verify the full story:

- syntax/build checks pass;
- app launches;
- menu/tray entry appears;
- shortcut hint appears on open;
- first-launch simulation is available and uses a non-editable prompt;
- Smart Arrange moves real windows or uses a safe dry-run if moving windows would disrupt the user;
- Save Current Desk writes a local snapshot;
- Pick Saved Desk can read and apply a saved snapshot;
- Explain Inferred Desk shows the inferred roles;
- Create Desktop Launch Shortcut creates the OS-native launcher and it opens the app;
- Export Blueprint/Spec produces a public-safe file with no personal data;
- no network is required for normal operation.

## Completion Report

Finish with:

- the app location;
- how to launch it;
- where the desktop/start-menu launcher was created, if created;
- where local config is stored;
- what was verified;
- any permission the user still needs to grant.

Success means a non-technical first-time user can open DeskGrid, understand the available commands, get a useful smart layout in one action, save/restore desks, create an obvious launcher, and never think about the implementation stack.
````
