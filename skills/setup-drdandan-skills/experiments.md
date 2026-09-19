# Performance experiment organization

Apply this workflow before creating, naming, or reorganizing a performance
experiment.

## 1. Confirm the experiment path

Propose a descriptive experiment directory and obtain user confirmation before
creating it. Default to:

```text
experiments/<confirmed-experiment-name>/
```

This step is complete only after both the name and absolute path are confirmed.

## 2. Create the experiment structure

Create these four required subdirectories and one root metadata file:

```text
experiments/<confirmed-experiment-name>/
|-- codebase/
|-- numerical_results/
|-- illustrations/
|-- tests/
`-- README.md
```

Place files by responsibility:

- Put experiment-specific runnable code and configuration in `codebase/`.
- Put reusable production code under the repository root `src/` directory created
  during setup, not under `experiments/`; experiment code may import it.
- Keep source code and configuration out of the experiment root. Reserve the root
  for `README.md` and the four required subdirectories.
- Put numerical outputs, summary tables, run manifests, and status records in
  `numerical_results/`.
- Put figures derived from numerical results in `illustrations/`.
- Put test and debug scripts, fake adapters, smoke fixtures, and test-only data in
  `tests/`.

The setup workflow creates the root `.gitignore` with one copy of each rule below.
Before running an experiment, preserve these rules and restore either one if it is
missing:

```gitignore
experiments/*/numerical_results/
experiments/*/illustrations/
```

Run `git check-ignore -v --no-index` for both directories. This step is complete
when every experiment file has a defined location, all four subdirectories exist,
the experiment root contains no source or configuration files, and Git ignores
both output directories.

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
- The root `.gitignore` covers both output directories and Git confirms the rules.
- The root `README.md` maps every formal figure to its numerical source.

Report the confirmed experiment path, the four required subdirectories, and the
README provenance mapping location.
