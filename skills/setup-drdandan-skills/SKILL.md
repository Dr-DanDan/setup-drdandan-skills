---
name: setup-drdandan-skills
description: "Initialize a repository for the DrDanDan engineering skills: Git identity, source and experiment layout, agent guidance, issue tracking, domain docs, task tracking, and optional DDDvis report-publishing binding. Run before first use or when updating these project conventions."
---

# Setup DrDanDan Skills

Scaffold the per-repo configuration that the engineering skills assume:

- **Repository bootstrap**: initialize Git, confirm the repository-local author identity, and ensure root `src/` and `.gitignore` paths exist
- **Issue tracker**: where issues live (GitHub by default; local markdown is also supported out of the box)
- **Triage labels**: the strings used for the five canonical triage roles
- **Domain docs**: where `CONTEXT.md` and ADRs live, and the consumer rules for reading them
- **Scope limits**: proportionate fixes and verification without suppressing reachable defects or required project checks
- **Performance experiments**: choose a new or existing experiment directory, with a fixed code, results, illustrations, tests, logs, and provenance layout under `experiments/`
- **Task tracking**: a root `TODO.md` ledger for current tasks, priority, and development progress
- **Feishu ticket transfer**: local sync conventions for the paired `lark-push-tickets` and `lark-pull-tickets` skills; bind the remote project only when first used
- **DDDvis website reports (optional)**: a project skill and binding to an existing shared preview service, with the current URL read from service-owned state

This is a prompt-driven skill, not a deterministic script. Explore, present what you found, confirm with the user, then write.

## Process

### 1. Explore

Look at the current repo to understand its starting state. Read whatever exists; don't assume:

- Git state, `git remote -v`, and `.git/config`: is the target root already a Git repository, which remote does it use, and which local or effective `user.name` and `user.email` values are available?
- Root `src/` and `.gitignore`: do they exist, and which existing ignore rules must be preserved?
- Root `AGENTS.md`: does it exist, and does it already contain an `## Scope limits` or `## Agent skills` section?
- The README, security guidance, supported interfaces, and real-data documentation: what repository role and operator or threat model do they establish?
- Existing provenance, hashing, migration, validation, review, experiment, and long-running-job conventions: which are explicit project contracts that scope limits must preserve?
- `experiments/`, experiment guidance, and matching `.gitignore` rules: what layout already exists, and what conflicts with the required structure in [experiments.md](./experiments.md)?
- `CONTEXT.md` and `CONTEXT-MAP.md` at the repo root
- `docs/adr/` and any `src/*/docs/adr/` directories
- `docs/agents/`: does this skill's prior output already exist?
- `TODO.md`: does a task ledger already exist, and which priority or progress vocabulary does it use?
- `.scratch/`: inspect existing specs/issues for a local-markdown tracker convention; `.scratch/.feishu-sync/` is only transfer state and does not select the tracker
- `docs/agents/feishu-sync.md`: preserve any existing project binding and setup-declared context paths; do not require Feishu login during setup
- `.agents/skills/dddvis/`, `docs/agents/dddvis.md`, and any existing DDDvis entry in `AGENTS.md`: inspect prior binding and user edits. If website publishing is requested or already configured, read the referenced service configuration to identify its publisher, project registry, current-entry record, and supported report/recovery capabilities. Setup does not need credentials or a new tunnel.
- Is the `triage` skill installed? (a `triage` skill folder alongside this one, or `triage` in your available skills.) This decides whether Section C runs at all.
- Monorepo signals: a `pnpm-workspace.yaml`, a `workspaces` field in `package.json`, or a populated `packages/*` with its own `src/`. These are present only in a genuinely large multi-package repo; their absence means single-context, which is almost every repo.

### 2. Present findings and ask

Summarise what's present and what's missing. Then take the sections in order. One section, one answer, then the next.

Lead each section with the recommended answer so the user can accept it in a word. Give a one-line explainer only when the choice genuinely branches; skip the section entirely when exploration already settled it (Section C when `triage` isn't installed, Section D when there's no monorepo).

**Section A: Repository bootstrap.**

Always ask one question that confirms both Git identity values before writing:

> Use `user.name = <name>` and `user.email = <email>` for this repository? If not, provide both replacement values.

Recommend the existing repository-local values when both are present; otherwise recommend the effective Git values when both are available. When either value is unavailable, ask the user to supply both. State that the confirmed values will be written to this repository's local Git config, not the global config. Also state whether setup will initialize Git, create root `src/`, and create or update root `.gitignore`.

**Section B: Issue tracker.**

> Explainer: The "issue tracker" is where issues live for this repo. Skills like `to-tickets`, `triage`, and `to-spec` read from and write to it. They need to know whether to call `gh issue create`, write a markdown file under `.scratch/`, or follow some other workflow you describe. Pick the place you actually track work for this repo.

Default posture: these skills were designed for GitHub. If a `git remote` points at GitHub, propose that. If a `git remote` points at GitLab (`gitlab.com` or a self-hosted host), propose GitLab. Otherwise (or if the user prefers), offer:

- **GitHub**: issues live in the repo's GitHub Issues (uses the `gh` CLI)
- **GitLab**: issues live in the repo's GitLab Issues (uses the [`glab`](https://gitlab.com/gitlab-org/cli) CLI)
- **Local markdown**: issues live as files under `.scratch/<feature>/` in this repo (good for solo projects or repos without a remote)
- **Other** (Jira, Linear, etc.): ask the user to describe the workflow in one paragraph; the skill will record it as freeform prose

Record the choice in `docs/agents/issue-tracker.md`. The GitHub and GitLab templates carry a "PRs as a request surface" flag, defaulted **off**. Leave it off and don't raise it: a user who wants external PRs in the triage queue can flip the flag in the file later.

**Section C: Triage label vocabulary.** Skip this section entirely if the `triage` skill isn't installed (exploration told you), since an uninstalled skill needs no labels.

If it is installed, ask exactly one question:

> Do you want to keep the default triage labels? (recommended: **yes**)

The defaults are the five canonical roles, each label string equal to its name: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. On **yes**, write them as-is. Only if the user says no, usually because their tracker already uses other names (e.g. `bug:triage` for `needs-triage`), collect the overrides so `triage` applies existing labels instead of creating duplicates.

**Section D: Domain docs.** Default to **single-context** (one `CONTEXT.md` + `docs/adr/` at the repo root). This fits almost every repo; write it without asking.

Offer **multi-context** (a root `CONTEXT-MAP.md` pointing to per-context `CONTEXT.md` files) only when exploration found monorepo signals. Then confirm which layout they want.

**Section E: DDDvis website reports (optional).** Preserve an existing choice or one already authorized in this task. Otherwise ask whether to bind this project to an existing shared preview service; recommend binding when the user wants browser reports, and leave it off when they do not. If declined, add no DDDvis files or routing and leave any existing configuration intact.

For a selected binding, use [dddvis.md](./dddvis.md). Resolve the project identifier and stable service-configuration path from existing records; ask only for missing information. A temporary URL is not a service binding. If no service is available, the project may be prepared as unbound, but report that publication and recovery are unavailable until a service is provisioned separately. Creating a tunnel or enabling a watchdog is outside setup.

### 3. Confirm and edit

Show the user a draft of:

- The repository bootstrap changes: Git initialization status, confirmed local identity, root `src/` creation when needed, and the merged root `.gitignore`
- The `## Scope limits` block, adapted from [scope-limits.md](./scope-limits.md) to the repository facts found in step 1
- The `## Agent skills` block to add to root `AGENTS.md`
- The root `TODO.md`, preserving existing tasks and conventions when present
- The contents of `docs/agents/experiments.md`, `docs/agents/issue-tracker.md`, `docs/agents/domain.md`, `docs/agents/feishu-sync.md`, and `docs/agents/triage-labels.md` (the last only when `triage` is installed)
- If DDDvis was selected: its project binding, the project skill files, and the conditional `AGENTS.md` route; distinguish verified service capabilities from pending ones

Resolve every project-specific slot in the scope-limits seed; never show or write literal placeholders. State any inferred operator model or preserved project contract in the draft so the user can correct it. Let them edit before writing.

### 4. Write

**Bootstrap the repository:**

- Initialize Git in the target root when it is not already a Git working tree. Preserve an existing repository.
- Write the user-confirmed identity with `git config --local user.name` and `git config --local user.email`; do not modify global Git configuration.
- Create the root `src/` directory when it is absent. Preserve its existing contents and do not move existing code into it.
- Create the root `.gitignore` when it is absent. Preserve existing rules and ensure exactly one copy of each performance-output rule is present:

```gitignore
experiments/*/numerical_results/
experiments/*/illustrations/
.scratch/.feishu-sync/
experiments/*/log/
```

Verify that the target root is a Git working tree, both local identity values match the confirmation, root `src/` and `.gitignore` exist, and `git check-ignore -v --no-index` matches representative paths under all three ignored experiment-output directories.

**Edit `AGENTS.md`:**

Also verify that `git check-ignore -v --no-index` matches `.scratch/.feishu-sync/state.json`. This cache rule does not ignore ordinary `.scratch/` tickets.

- Update root `AGENTS.md` when it exists; otherwise create it.

Use [scope-limits.md](./scope-limits.md) as the seed for the approved `## Scope limits` block in root `AGENTS.md`. If that section already exists, merge only the approved missing rules into it and preserve user-specific wording. Otherwise insert it immediately before `## Agent skills`, or near the top when no `## Agent skills` section exists.

If an `## Agent skills` block already exists in root `AGENTS.md`, update its contents in-place rather than appending a duplicate. Don't overwrite user edits to the surrounding sections. Root `AGENTS.md` must contain exactly one `## Scope limits` section and one `## Agent skills` section when writing is complete.

The block:

```markdown
## Agent skills

### Performance experiments

When the user intends to develop, run, debug, or reorganize an experiment, follow `docs/agents/experiments.md` to establish the new or existing experiment directory before starting work.

### Issue tracker

[one-line summary of where issues are tracked]. See `docs/agents/issue-tracker.md`.

### Triage labels

[one-line summary of the label vocabulary]. See `docs/agents/triage-labels.md`.

### Domain docs

[one-line summary of layout: "single-context" or "multi-context"]. See `docs/agents/domain.md`.

### Feishu ticket transfer

When uploading project tickets to Feishu, use `lark-push-tickets`; when importing them, use `lark-pull-tickets`. Read `docs/agents/feishu-sync.md` for the project binding, HITL selection, dependency and conflict rules.

### Task tracking

Use the root `TODO.md` as the current-work ledger. Record each active task's priority and development progress, and update it when its state changes.
```

Include the `### Triage labels` sub-block, and write `docs/agents/triage-labels.md`, only when `triage` is installed and Section C ran. When it isn't, both are omitted.

When DDDvis was selected, merge this sub-block into the same `## Agent skills` section once:

```markdown
### DDDvis website reports

When the user explicitly invokes `DDDvis` to generate or publish a website report, follow `.agents/skills/dddvis/SKILL.md` and the binding in `docs/agents/dddvis.md`. Ordinary HTML and `visualize` requests do not publish to the website.
```

Then write the docs files using the seed templates in this skill folder as a starting point:

- [scope-limits.md](./scope-limits.md): project-adapted fix and verification boundaries for root `AGENTS.md`
- [experiments.md](./experiments.md): performance-experiment naming, layout, placement, provenance, and acceptance rules
- [issue-tracker-github.md](./issue-tracker-github.md): GitHub issue tracker
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md): GitLab issue tracker
- [issue-tracker-local.md](./issue-tracker-local.md): local-markdown issue tracker
- [triage-labels.md](./triage-labels.md): label mapping (only if `triage` is installed)
- [domain.md](./domain.md): domain doc consumer rules + layout
- [todo.md](./todo.md): root `TODO.md` task-ledger template
- [feishu-sync.md](./feishu-sync.md): paired ticket-transfer conventions and initially unbound Feishu project configuration
- [dddvis.md](./dddvis.md): optional project publishing binding and shared-service contract; only write when Section E selected it

Create `docs/agents/feishu-sync.md` from its seed when absent. When it exists, preserve its binding and user edits while adding missing transfer rules. For a multi-context layout, record only the `src/<context>/CONTEXT.md` paths already established by the domain configuration in its `context_paths` list. Do not change the issue tracker, create Feishu resources, or request remote authentication during setup.

For "other" issue trackers, write `docs/agents/issue-tracker.md` from scratch using the user's description.

Create `TODO.md` at the repository root from [todo.md](./todo.md). If it already exists, preserve its tasks and local conventions while ensuring each active task records its priority and progress. Populate it with active work found during exploration or supplied by the user; don't invent tasks when none are known.

**Bind DDDvis only when selected:**

- Create `docs/agents/dddvis.md` from its seed, filling the known project identifier and service-configuration path. Retain `null` only for an explicitly unbound setup. Preserve existing bindings and user edits on reruns; reconcile a conflicting binding before changing it.
- Copy [assets/dddvis/SKILL.md](./assets/dddvis/SKILL.md) and [assets/dddvis/agents/openai.yaml](./assets/dddvis/agents/openai.yaml) into `.agents/skills/dddvis/`, retaining that directory structure. Merge missing instructions when these files already exist. Preserve the explicit-only invocation policy and keep the entire skill self-contained through its project-doc pointer.
- Reuse the bound service's project registration interface, if available, to add only this project. A registry entry is a directory label, not permission to publish its files. If registration cannot be verified, report it as pending rather than editing an unknown registry format.
- Verify the project skill, invocation policy, binding pointer, and single AGENTS route. Inspect service-owned records for actual capabilities and status; writing a recovery contract does not install or verify a recovery program. Keep passwords, live URLs and runtime records out of project files.

### 5. Done

Identify `docs/agents/feishu-sync.md` as the paired transfer skills' configuration. Its remote project can remain unbound until their first invocation. Both transfer skills must be installed together; setup does not install them automatically.

If DDDvis was selected, identify its project skill and binding, and report whether publication, static-resource bundles, and independent recovery are actually supported by the bound service. Return a current URL only when fresh service evidence confirms it; otherwise name the pending capability. Explain that tunnel recreation can change the hostname and that old chat links and browser bookmarks will not update automatically.

Tell the user the setup is complete, whether Git was initialized, which repository-local Git identity was configured, and whether root `src/` and `.gitignore` were created or updated. State that root `AGENTS.md` received `## Scope limits` and the `TODO.md` maintenance rule, and identify `TODO.md` as the current-work ledger and `docs/agents/experiments.md` as the experiment-layout source of truth. Say which engineering skills will now read from the generated files. They can edit `TODO.md` and `docs/agents/*.md` later, or rerun setup to change project conventions or bindings; ordinary DDDvis publication does not rerun setup.
