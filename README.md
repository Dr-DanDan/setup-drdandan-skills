# Setup DrDanDan Skills

A Codex skill that initializes a repository for the DrDanDan engineering
workflow: Git identity, `src/`, `.gitignore`, agent guidance, experiment layout,
issue tracking, triage labels, domain documentation, and `TODO.md`.

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
