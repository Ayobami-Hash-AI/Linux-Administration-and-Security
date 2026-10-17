# Lab 003 — Project Directory Structure and Verification

**Platform:** LabEx · The Digital Architect  
**Date:** October 16, 2026  
**Status:** Directory-structure task verified; remaining lab tasks not yet evidenced.

## Objective

Organize `~/project/phoenix_project` with `src`, `config`, and `docs` subdirectories for source code, configuration, and documentation.

## Actions performed

I inspected the project directory, navigated into `phoenix_project`, and created the requested subdirectories. I initially named the documentation directory `doc`, identified the mismatch, removed the empty directory, and created `docs`. I verified the corrected names with `ls -F`.

```bash
ls
cd phoenix_project
mkdir src
mkdir config
mkdir doc
ls -F
rmdir doc
mkdir docs
ls -F
```

## Verified result

The final listing shows:

```text
README.md  config/  config.json  docs/  main_app.py  src/
```

| Directory | Intended purpose |
|---|---|
| `src/` | Source code |
| `config/` | Configuration files |
| `docs/` | Documentation |

The existing `main_app.py`, `config.json`, and `README.md` remain at the project root in this evidence. Moving files into the new directories has not yet been demonstrated.

## Evidence

![LabEx task specifying src, config, and docs directories](screenshots/labex-project-directory-task.png)

*Figure 1: Required project directory structure.*

![Initial directory creation showing doc instead of docs](screenshots/labex-initial-directory-creation.png)

*Figure 2: Initial creation and listing before the naming correction.*

![Corrected project listing confirming src, config, and docs](screenshots/labex-corrected-directory-structure.png)

*Figure 3: Final directory verification after correcting the documentation directory.*

## Troubleshooting and lessons learned

- Exact names matter: `doc` and `docs` are different paths.
- `rmdir` removes empty directories; it succeeded for the unused `doc` directory.
- `ls -F` adds classification indicators, including a trailing slash for directories.
- A consistent directory layout supports maintainability and prepares for deliberate access-control configuration. No ownership or permission changes are evidenced in this step.

## Next step

Continue the next LabEx task and capture its commands and verification before marking additional work complete.

[← Linux portfolio index](../../README.md)
