# Codex History Browser Goal Prompt

Use this prompt when Codex Desktop, Codex CLI, or another Codex interface only
shows a small number of recent chats/sessions, but older local session history
may still exist on disk.

The prompt is intentionally platform-agnostic. It asks the executing agent to
discover the current app, operating system, local storage format, and safest
access path before changing anything.

```md
/goal You are enabling access to all locally available Codex chat/session history to maximum verified completion.

Mission:
Make it possible to browse or search all Codex sessions/chats, not just the small subset visible in the sidebar or recent-session list.

Raw objective:
The Codex interface only shows a limited number of sessions/chats for a project or workspace. This is a blocker for users who run many sessions and need to find recent or older work, templates, decisions, code discussions, or notes. Success means there is a reliable way to browse or search all available Codex history, whether through the app UI or a safe local archive/search tool.

Operating rules:
- Work locally and preserve user data.
- Start read-only. Locate where Codex stores session/chat metadata and transcripts before changing anything.
- Do not delete, truncate, migrate, overwrite, or corrupt application state.
- Before modifying any Codex app data, create a timestamped backup of every touched file or database.
- If modifying Codex internals is risky, opaque, unsupported, or brittle, build a read-only sidecar browser/search tool instead.
- Prefer safe, durable, platform-appropriate local tooling over invasive app changes.
- Do not rely on private APIs, credentials, cloud access, or undocumented destructive behavior.
- Do not upload private chats anywhere.
- Do not expose secrets, tokens, personal identifiers, private project names, private repository names, or private chat content in generated artifacts.
- If older sessions are not retained locally, prove that with evidence and provide the best available recovery, export, or future-archiving path.

Scope:
- Discover where Codex stores local session/chat data.
- Count all available sessions, grouped by project/workspace if possible.
- Extract useful metadata when available: title, timestamp, project/workspace, model, and transcript location.
- Provide a way to browse and search beyond the visible sidebar or recent-session limit.
- Make the result reusable, not a one-off terminal dump.

Non-scope:
- Do not patch signed application bundles unless explicitly approved by the user.
- Do not disable app updates.
- Do not upload private chats, logs, databases, or transcripts to external services.
- Do not publish generated indexes or search pages that contain private content.
- Do not create a fragile manual checklist as the final solution if a durable local tool is feasible.

Execution phases:
1. Discover current truth.
   Verify: app/interface version, operating system, local storage paths, data formats, and whether the visible history limit is UI-only or storage-level.
2. Inventory available sessions.
   Verify: count all locally available sessions and confirm whether sessions beyond the visible limit exist.
3. Choose the safest access path.
   Verify: decide between an app-level fix, read-only archive browser, CLI search, local HTML page, or another local-only tool.
4. Implement the access tool or fix.
   Verify: no unrelated changes; backups exist if any app files were touched.
5. Test search and browsing.
   Verify: search for a known phrase from an older session and open/read the matching transcript locally.
6. Make it reusable.
   Verify: provide a stable command, local file, or local URL the user can use again later.
7. Final handoff.
   Verify: report exactly what was found, what was built, where it lives, and any limitations.

Completion standard:
- Do not declare success until the user has a working way to browse or search all locally available Codex sessions beyond the visible limit.
- Report separately:
  - App UI state: fixed, unchanged, or not safely modifiable.
  - Archive/search state: working or blocked.
  - Session count: total discovered and searchable.
  - Data safety: backups created, if any files were touched.
  - Durability: how to use the solution again.

Final response must include:
- What changed or was built.
- Exact local path, command, or URL for browsing/searching history.
- Number of sessions discovered.
- Whether sessions beyond the visible limit were found.
- Backup paths for any touched files.
- Any remaining blockers and the exact next step.
```

## Safety Notes

This prompt intentionally prefers read-only discovery and sidecar tooling over
modifying application internals. Chat histories often contain private material,
credentials, personal data, unpublished code, and project context. Any generated
index or browser should stay local unless the user explicitly redacts and
chooses to publish it.
