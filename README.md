# goal-prompts

Portable, paste-ready goal prompts and prompt-publishing workflows for Codex
and other autonomous coding agents.

These prompts are written as execution contracts: they define the mission,
scope, non-scope, operating rules, verification requirements, and completion
standard so an agent can drive a task to a real result instead of stopping at
advice.

## Prompts

- **[codex-history-browser.md](codex-history-browser.md)** - build or find a
  safe way to browse and search all locally available Codex chat/session
  history, beyond a short sidebar limit.
- **[deskgrid-window-tiler.md](deskgrid-window-tiler.md)** - build a
  local-first, OS-adaptive window tiling app with smart arrange, saved desks,
  and a non-technical launch shortcut.
- **[prompt-publish.md](prompt-publish.md)** - a portable skill specification
  for turning private/local goal prompts into public-safe prompt files and
  publishing them to a prompt repository.

## What "portable" means here

These are public-safe prompt specifications, not private workflows. They:

- avoid local usernames, machine names, organization names, private projects,
  secrets, credentials, tickets, and absolute paths;
- avoid assuming a specific operating system, filesystem layout, shell, account,
  or repository;
- prefer reversible, local, read-only discovery before changes;
- require backups before touching application state;
- define evidence-based completion criteria;
- can be pasted into a coding agent as-is, then adapted by the executing agent
  to the user's current environment.

## How to use

Open a prompt file, copy the Markdown block, and paste it into your coding
agent.

If you are using Codex and want Codex goal mode, prefix the first instruction
with `/goal`. For other agents, paste the prompt as a normal instruction and
keep the same completion criteria.

## Contributing

Pull requests welcome. Keep prompts:

- self-contained;
- public-safe;
- platform-agnostic unless the prompt is intentionally platform-specific;
- focused on observable completion rather than vague advice;
- explicit about destructive actions, backups, and final evidence.

## License

MIT. See [LICENSE](LICENSE).
