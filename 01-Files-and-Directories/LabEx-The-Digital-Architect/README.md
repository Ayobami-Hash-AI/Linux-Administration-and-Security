# Lab 003 — Project Directory Structure and Verification

**Platform:** LabEx · The Digital Architect  
**Date:** October 16, 2026  
**Status:** Directory creation and file-move commands evidenced; project-root listing verified. Destination contents have not yet been listed.

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

At this initial stage, `main_app.py`, `config.json`, and `README.md` remained at the project root. The next section records their subsequent organization.

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

## Organizing existing project files

I moved each existing project file into its designated directory and inspected the project root again.

```bash
mv main_app.py src/
mv config.json config/
mv README.md docs/
ls -F
```

The final root listing shows:

```text
config/  docs/  src/
```

| Source file | Destination specified |
|---|---|
| `main_app.py` | `src/main_app.py` |
| `config.json` | `config/config.json` |
| `README.md` | `docs/README.md` |

The terminal shows no errors from the three `mv` commands, and the files no longer appear in the root listing. A listing inside each destination is still needed for direct confirmation of its contents.

![LabEx instructions for moving project files to designated directories](screenshots/labex-project-file-organization-task.png)

*Figure 4: Required source-code, configuration, and documentation destinations.*

![Terminal showing three mv commands and a project root containing config, docs, and src](screenshots/labex-project-files-moved-root-verification.png)

*Figure 5: File-move commands and resulting project-root listing.*

**Command interpretation:** `mv source directory/` relocates a file into that directory. Unlike `cp`, it does not leave a second copy at the original path. The trailing slash makes the directory destination explicit.

**Follow-up verification (suggested, not yet evidenced):**

```bash
ls -l src/ config/ docs/
```

This exercise follows the LabEx requirement to place `README.md` in `docs/`. The GitHub lab report remains in this project's root so visitors can read it directly.

## Next step

Continue the next LabEx task and capture its commands and verification before marking additional work complete.

[← Linux portfolio index](../../README.md)
