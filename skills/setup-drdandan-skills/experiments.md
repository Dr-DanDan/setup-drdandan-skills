# Performance experiment organization

Apply this workflow when the user intends to develop, run, debug, or reorganize
an experiment. Ordinary application-code changes do not trigger this workflow.

## 1. Confirm the experiment path

Before starting an experiment task, inspect existing directories under
`experiments/` and ask whether to create a new directory or reuse an existing
one. List the existing choices. For a new experiment, propose a descriptive name
and absolute path under:

```text
experiments/<confirmed-experiment-name>/
```

An explicit user-specified directory counts as the choice; resolve and report its
absolute path without asking again. Otherwise, wait for the user's selection
before creating files or starting the experiment. Reuse the confirmed directory
throughout the same task; ask again only when switching experiments and the new
target is unspecified.

This step is complete when the new or existing directory and its absolute path
are established.

## 2. Create the experiment structure

Ensure these five required subdirectories and one root metadata file exist.
When reusing an experiment, create only missing entries and preserve existing
files; flag layout conflicts before moving or overwriting anything.

```text
experiments/<confirmed-experiment-name>/
|-- codebase/
|-- numerical_results/
|-- illustrations/
|-- tests/
|-- log/
`-- README.md
```

Place files by responsibility:

- Put experiment-specific runnable code and configuration in `codebase/`.
- Put reusable production code under the repository root `src/` directory created
  during setup, not under `experiments/`; experiment code may import it.
- Keep source code and configuration out of the experiment root. Reserve the root
  for `README.md` and the five required subdirectories.
- Put numerical outputs, summary tables, run manifests, and configuration records in
  `numerical_results/`.
- Put figures derived from numerical results in `illustrations/`.
- Put test and debug scripts, fake adapters, smoke fixtures, and test-only data in
  `tests/`.
- Put long-running task logs, progress, timestamps, and failure records in `log/`.
  Use a distinct run identifier in log filenames or subdirectories so later runs
  preserve earlier logs. The running program must persist these records itself.

The setup workflow creates the root `.gitignore` with one copy of each rule below.
Before running an experiment, preserve these rules and restore any rule that is
missing:

```gitignore
experiments/*/numerical_results/
experiments/*/illustrations/
experiments/*/log/
```

Run `git check-ignore -v --no-index` for all three ignored directories. This step is complete
when every experiment file has a defined location, all five subdirectories exist,
the experiment root contains no source or configuration files, and Git ignores
the numerical, illustration, and log directories.

## 3. Record figure provenance

Maintain the mapping between each formal figure and its numerical source in the
experiment root `README.md`:

| Figure | Numerical source | Run identifier or configuration |
|---|---|---|
| `illustrations/<figure>` | `numerical_results/<source>` | `<run-id-or-config>` |

Use directly resolvable relative paths. List every output format, or explicitly
state the shared basename when one numerical result produces several formats.
This step is complete when every formal figure maps to exactly one numerical
source and run identity.

## 4. Accept the organization

Finish only when:

- The experiment name and path have user approval.
- Experiment-specific code and configuration are under `codebase/`; reusable
  production code remains under the repository root `src/` directory.
- Numerical products appear only in `numerical_results/`.
- Figures appear only in `illustrations/`.
- Tests and debugging support appear only in `tests/`.
- Long-running task logs and progress records appear in `log/`, separated by run.
- The root `.gitignore` covers the numerical, illustration, and log directories,
  and Git confirms the rules.
- The root `README.md` maps every formal figure to its numerical source.

Report the confirmed experiment path, the five required subdirectories, and the
README provenance mapping location.
