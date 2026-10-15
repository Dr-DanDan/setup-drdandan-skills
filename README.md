# Setup DrDanDan Skills

[English](README.md) | [简体中文](README.zh-CN.md)

A Codex skill that initializes a repository for the DrDanDan engineering
workflow: Git identity, `src/`, `.gitignore`, agent guidance, experiment layout,
issue tracking, triage labels, domain documentation, and `TODO.md`.

## What it configures

- Repository-local Git identity, root `src/`, and `.gitignore`.
- Scope limits and workflow entry points in `AGENTS.md`.
- GitHub, GitLab, local Markdown, or a user-defined issue tracker.
- Triage labels when the `triage` skill is installed.
- Rules for reading domain context and architecture decision records (ADRs).
- A root `TODO.md` ledger for task priority and progress.
- Experiment directory selection, file placement, logs, and figure provenance.
- Feishu ticket-transfer conventions and an initially unbound mother-document configuration for the paired `lark-push-tickets` / `lark-pull-tickets` skills.
- Optional `DDDvis` project skill and binding to an existing shared HTML preview service.

Before experiment development, execution, debugging, or reorganization, the
generated guidance asks whether to create a directory or reuse an existing one
under `experiments/`. An explicitly specified or already confirmed directory is
reused without repeated questions. Ordinary application-code changes do not
trigger this step.

```text
experiments/<experiment-name>/
|-- codebase/           # Experiment code and configuration
|-- numerical_results/ # Numerical outputs, tables, and run manifests
|-- illustrations/     # Figures derived from results
|-- tests/             # Tests and debugging support
|-- log/               # Long-running task logs and progress records
`-- README.md          # Figure-to-source and run mapping
```

Reusable production code belongs in root `src/`. Numerical results, illustrations,
and logs are ignored by Git. The running program persists logs, timestamps,
progress, and failures in `log/`, separated by run to preserve earlier records.

This is a prompt-driven setup skill: inspect the repository, confirm choices,
present a draft, write, and verify. It preserves existing files and conventions.
Domain documents and actual experiments are created when needed; setup records
their rules in `docs/agents/`.

## Optional DDDvis binding

When selected during setup, DDDvis adds `.agents/skills/dddvis/`, a conditional
AGENTS entry and `docs/agents/dddvis.md`. Only an explicit `DDDvis` request
publishes a report; ordinary HTML generation and `visualize` keep their own
delivery paths. Rerunning setup preserves the binding and user edits.

Projects share an existing server while retaining their own report grouping.
The binding points to stable service configuration; the current URL comes from
service-owned state, so temporary hostnames are not copied into repository rules.
Reports can be self-contained HTML or static resource bundles, with original
artifacts retained and updates keeping their report paths.

The included service contract covers independent, bounded recovery: 60-second
checks, three consecutive failures, at most two repairs, then a 600-second
cooldown, with manual stops respected. **This repository supplies setup guidance
and a project skill template, not a web server, tunnel or recovery daemon.**
Setup reports unsupported service capabilities; it does not install them. Old
bookmarks and chat links do not automatically follow a recreated temporary URL.

## Install

```bash
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo Dr-DanDan/setup-drdandan-skills \
  --path skills/setup-drdandan-skills
```

Start a new Codex task after installation, then run:

```text
$setup-drdandan-skills
```

The skill asks for confirmation before changing the target repository.

## Requirements

- Codex with the built-in `$skill-installer`
- Git

## License and attribution

MIT licensed. This skill is derived from Matt Pocock's
[`setup-matt-pocock-skills`](https://github.com/mattpocock/skills/tree/5c89081d4bbeb3d039a42093653f90bb698d780e/skills/engineering/setup-matt-pocock-skills).
See [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md).
