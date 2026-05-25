# Prompt Publish Skill Specification

This document defines a reusable skill for publishing private or local goal
prompts as public, portable prompt specifications. It is intentionally
provider-agnostic, model-agnostic, account-agnostic, machine-agnostic,
repository-agnostic, and implementation-agnostic.

The skill can be implemented by any capable coding agent that can read a source
prompt, redact identifying material, rewrite it as a generic execution contract,
update a public prompt repository, and verify the result before publishing.

## Skill Identity

Name: `prompt-publish`

Purpose: convert a local `/goal` prompt, agent prompt, task prompt, workflow, or
chat-derived instruction into a public-safe prompt specification that another
user can paste into their own agent.

Use this skill when the user asks to:

- publish a goal prompt to a public prompt repository;
- make a private prompt public-safe;
- remove identifying information, secrets, local paths, account details, or
  infrastructure assumptions from a prompt;
- rewrite a prompt so it is platform-agnostic and reusable by other users;
- add a new prompt to a repository README or index;
- preserve the useful execution pattern while stripping private context.

## Non-Negotiable Constraints

- Never publish secrets, credentials, private keys, API tokens, account IDs,
  cookies, session IDs, credential URLs, private hostnames, local usernames,
  private organization names, private project names, private repository names,
  personal examples, absolute local paths, or private issue/PR/ticket IDs.
- Do not publish a prompt as if one user's local setup is universal.
- Do not hardcode provider, model, account, CLI, OS, repository, editor,
  browser, hosting provider, or path assumptions unless they are clearly marked
  as examples or placeholders.
- Make the public artifact a reusable prompt specification, not a dump of the
  private prompt or chat.
- Preserve the practical completion standard: observable success, evidence,
  rollback/backup rules where relevant, and exact final handoff.
- If the source prompt includes private examples, either remove them or replace
  them with neutral examples that do not identify people, companies, projects,
  repos, tickets, hosts, domains, or accounts.
- Commit and push only after redaction, formatting, README/index update, and
  verification pass.

## Required Inputs

The skill should work with these inputs:

- `source_prompt`: local prompt text, chat excerpt, file, workflow, or user
  instruction to publish.
- `target_repo`: public prompt repository. Default: discover the user's
  configured goal-prompt or prompt repository.
- `target_name`: public prompt name. Default: derive a concise kebab-case name
  from the source prompt's objective.
- `publication_style`: target repo conventions. Default: inspect existing files
  and match their structure.
- `example_policy`: whether to keep examples. Default: keep only neutral
  examples that clarify the prompt's use.
- `publish_mode`: direct push, branch and pull request, or local draft only.
  Default: follow the repository's existing workflow and branch protection.

## Required Outputs

Always produce:

- a public-safe prompt file in the target repository;
- any README, index, or catalog update required by repository style;
- a redaction summary;
- verification results;
- a git commit and push or pull request when the user asked to publish and the
  repository state permits it.

Do not modify unrelated files. If the target repository has unrelated dirty
changes, stop and report the conflict unless the user explicitly authorized a
safe workaround.

## Prompt Rewrite Rules

Public prompts should:

- begin with the exact command shape expected by the target repository when
  applicable, such as `/goal`;
- state the mission in one sentence;
- define observable completion criteria;
- separate scope, non-scope, assumptions, operating rules, and hard gates;
- require current-truth discovery before implementation;
- require evidence, not claims;
- distinguish local, development, production, and live acceptance states when
  relevant;
- prevent early stopping by making partial progress insufficient unless it
  satisfies the full mission;
- include an exact final handoff contract.

Public prompts should not:

- include personal anecdotes unless fully generalized;
- mention local machine names, private paths, private domains, internal
  services, private tickets, or private branch names;
- instruct users to bypass safety controls;
- upload private data, logs, transcripts, databases, or source code to external
  services by default;
- assume the user's app, shell, database, browser, package manager, or operating
  system.

## Workflow

1. Locate the target prompt repository.
2. Inspect its existing file format, tone, naming, README/index pattern, license,
   and publication conventions.
3. Check repository state. If unrelated dirty changes exist, block or isolate
   the work according to the user's instructions.
4. Read the source prompt or workflow.
5. Extract the transferable objective:
   - mission;
   - raw problem;
   - success criteria;
   - scope and non-scope;
   - safety gates;
   - execution phases;
   - final response requirements.
6. Remove or replace local-only material:
   - usernames, hostnames, paths, account names, project names;
   - private organizations, clients, domains, and repositories;
   - secrets and credential-like strings;
   - provider subscriptions, API keys, and internal CLI wrappers;
   - assumptions about one machine, branch, deploy target, or app build.
7. Rewrite the prompt as a portable execution contract that another user can
   paste into their own agent.
8. Add a short usage note above the prompt if the repository style supports it.
9. Update the target README or index.
10. Run redaction scans for secrets and identifiers.
11. Run formatting checks appropriate to the repository, at minimum Markdown
    readability and broken-link checks when available.
12. Review the diff.
13. Commit and push, or open a pull request, according to the target workflow.

## Redaction Checklist

Before publishing, scan for:

- private key blocks and credential formats;
- environment variables containing `key`, `token`, `secret`, `password`,
  `credential`, `cookie`, or `session`;
- URLs with embedded credentials;
- local usernames and home-directory paths;
- hostnames and machine names;
- private organization, client, project, repository, and branch names;
- real people used as private examples;
- account IDs, emails, phone numbers, messaging IDs;
- internal domains or deployment targets;
- private issue trackers, ticket IDs, PR URLs, or commit URLs;
- comments that reveal non-public incidents or operations.

Use placeholders such as `<user>`, `<repo>`, `<project>`, `<target_repo>`,
`<local path>`, `<service>`, `<ticket>`, and `[REDACTED_SECRET]` only when a
placeholder is clearer than removing the detail.

## Smoke Test

After drafting the public prompt, run these checks:

```text
Can a user with no private context understand what task this prompt is for?
```

Expected: yes, because the prompt explains the mission, scope, workflow,
verification, and final handoff.

```text
Can a capable agent execute the prompt without knowing the source user's setup?
```

Expected: yes, because the prompt requires current-truth discovery and avoids
hardcoded local assumptions.

```text
Does the prompt reveal private local details?
```

Expected: no. Any local details are removed, generalized, or replaced with
neutral placeholders.

```text
Does the target README or index list the new prompt?
```

Expected: yes, when the target repository maintains such an index.

## Publication Commands

Command shapes are illustrative. Use the host's safest equivalent:

```bash
git -C "<target_repo>" status --short
git -C "<target_repo>" diff --check
git -C "<target_repo>" diff
git -C "<target_repo>" add "<new_prompt_file>" "<index_file>"
git -C "<target_repo>" commit -m "Add <prompt-name> goal prompt"
git -C "<target_repo>" push origin "<branch>"
```

If the repository requires pull requests, create a branch and PR instead of
pushing directly to the protected branch.

## Failure Modes

Block publication when:

- redaction finds a secret or private identifier that cannot be safely removed;
- the target repo has unrelated dirty changes and no safe isolated path is
  authorized;
- the public rewrite still depends on private infrastructure;
- the prompt is advice-only and lacks observable completion criteria;
- the agent cannot verify what will be published.
