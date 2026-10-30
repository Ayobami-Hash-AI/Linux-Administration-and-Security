# Lab 006 — Team Access and Special Permissions

**Platform:** LabEx  
**Date:** October 30, 2026  
**Status:** In progress — current identity, Jack's account, and home-directory ownership and permissions verified.

## Objective

Build a small team environment using dedicated practice accounts to explore Linux identity, ownership, and access controls.

## Planned scope

- Create a practice user and manage primary and supplementary group membership.
- Secure a file and inspect a password lock.
- Control default permissions with `umask`.
- Configure sticky-bit and setgid directories.

All work is scoped to the lab environment. The exercise specifies leaving the default `labex` login path unchanged.

## Evidence

![LabEx introduction outlining team accounts, file permissions, umask, sticky bit, and setgid exercises](screenshots/labex-team-access-permissions-introduction.jpg)

*Exercise introduction and planned scope. This screenshot contains no executed commands or verified configuration changes.*

## Progress

| Stage | Evidence |
|---|---|
| Lab introduction | Captured |
| User and group configuration | Account and primary group verified; supplementary-group work pending |
| File permissions and password-lock inspection | Pending |
| Default permissions and special directory permissions | Pending |

Subsequent sections will record the commands performed, observed output, and access checks as the lab progresses.

## Inspecting identities and creating a practice account

I confirmed the current shell identity, created a dedicated practice account, and inspected its identity and home directory.

```bash
whoami
id
sudo useradd -m -U -s /bin/bash jack
id jack
ls -ld /home/jack
```

### Observed results

```text
labex
uid=5000(labex) gid=5000(labex) groups=5000(labex),27(sudo),121(ssl-cert),5002(public)
uid=5002(jack) gid=5003(jack) groups=5003(jack)
drwxr-x--- 2 jack jack 57 Oct 30 16:54 /home/jack
```

| Check | Verified result |
|---|---|
| Current shell user | `labex` |
| LabEx identity | UID `5000`, primary GID `5000`; listed groups `labex`, `sudo`, `ssl-cert`, and `public` |
| Jack's identity | UID `5002`, primary GID `5003`; listed group `jack` |
| Home directory | `/home/jack` exists and is owned by `jack:jack` |
| Home permissions | `750` (`rwxr-x---`): owner full directory permissions, group list/traverse, others no access through ordinary mode bits |

The `-m` option requests home-directory creation, `-U` creates a same-named primary group, and `-s /bin/bash` selects Bash as the login shell. The identity and directory checks confirm the account and home directory. The configured shell was requested but not independently queried in the displayed output.

A UID and a GID belong to separate namespaces: Jack's UID `5002` matching the numeric GID of `public` does not make Jack a member of that group.

### Screenshots

![LabEx instructions for inspecting the current shell identity](screenshots/labex-identity-inspection-task.jpg)

*Current-user and identity inspection requirements.*

![LabEx instructions for creating jack with a home directory, primary group, and Bash shell](screenshots/labex-jack-account-creation-task.jpg)

*Practice-account creation and verification requirements.*

![Terminal confirming labex identity, jack creation, and home-directory ownership and permissions](screenshots/labex-jack-identity-home-verification.jpg)

*Executed commands and observed verification results.*

**Verification scope:** No password configuration, login attempt, supplementary-group assignment for Jack, or file-access test is shown yet. A follow-up `getent passwd jack` can confirm the configured shell without exposing authentication data.

[← Linux portfolio index](../../README.md)
