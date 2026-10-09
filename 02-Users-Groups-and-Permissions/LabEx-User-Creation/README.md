# Lab 002 — Creating and Verifying a Linux User Account

**Platform:** LabEx  
**Date:** October 9, 2026  
**Topic:** Linux user administration and account verification  
**Status:** Account creation, Bob's home-directory permissions, Joker's password update, home-path modification, Bash shell configuration, sudo-group membership, interactive login, and privileged command execution evidenced.

## Objective

Create and verify local Linux accounts, configure a password, modify a home-directory path, and change a login shell.

## Environment

Interactive Linux terminal in the LabEx learning environment. The terminal prompt shown is `labex:project/`.

## Observed commands

```bash
sudo useradd joker
ls
sudo grep -w 'joker' /etc/passwd
```

**Observed account entry:**

```text
joker:x:5001:5001::/home/joker:/bin/sh
```

## Step-by-step analysis

1. **`sudo useradd joker`** requests creation of the local account `joker` with elevated privileges. The screenshot shows no error message after this command.
2. **`ls`** lists the current directory; the screenshot does not show any directory entries in its output. This is not an account-verification command.
3. **`sudo grep -w 'joker' /etc/passwd`** searches the local account database for the username. The returned entry confirms that an account record named `joker` exists.

### Understanding the `/etc/passwd` record

| Field | Value | Meaning |
|---|---|---|
| Username | `joker` | Local login name |
| Password placeholder | `x` | Password hashes are normally stored separately in `/etc/shadow` |
| UID | `5001` | Numeric user identifier |
| Primary GID | `5001` | Numeric primary group identifier |
| GECOS | empty | Optional descriptive account information |
| Home path | `/home/joker` | Configured home directory path |
| Login shell | `/bin/sh` | Configured shell |

**Important distinction:** A home directory path appearing in `/etc/passwd` does **not** prove that the directory exists. The initial screenshot does not demonstrate password configuration or interactive login; the password update is evidenced later in this report.

## Security significance

Local accounts are security principals. Their UIDs, group memberships, authentication configuration, and login shells affect system access and auditability. Good account management supports least privilege and accountable access.

## Verification to perform next

```bash
id joker
getent passwd joker
ls -ld /home/wayne
sudo passwd -S joker
```

These are **suggested follow-up commands**, not commands shown as completed in the screenshot. Do not publish password hashes or credentials.

## Evidence

- **Source:** User-shared LabEx screenshot from October 9, 2026 showing the account-creation command and `/etc/passwd` output.

![LabEx terminal showing joker account creation and verification in /etc/passwd](screenshots/labex-user-creation-joker-account-verification.png)

*Figure 1: Creating `joker` and confirming the account record in `/etc/passwd`.*

**Initial evidence scope:** Account record creation only. Subsequent evidence below covers Bob's home directory and Joker's password and account-property changes.


## Progress update — creating a user with a home directory

A subsequent LabEx screenshot shows the following commands:

```bash
sudo useradd -m bob
sudo ls -ld /home/bob
```

Observed output:

```text
drwxr-x--- 2 bob bob 57 Oct 10 06:44 /home/bob
```

**What this establishes:** The `-m` option requested creation of a home directory for the new account. The `ls -ld` result confirms that `/home/bob` exists as a directory, is owned by user `bob` and group `bob`, and has permission mode `750` (`rwxr-x---`). The listing shows link count `2` and directory size `57` bytes in this environment. The date/time in the terminal is the lab environment's timestamp, not necessarily the user's local time.

**Permissions interpretation:**

| Identity | Permissions | Meaning |
|---|---|---|
| Owner `bob` | `rwx` | List, create/remove entries (subject to other controls), traverse |
| Group `bob` | `r-x` | List and traverse, but not modify directory entries |
| Others | `---` | No access via ordinary Unix mode bits |

**Security relevance:** Creating an account with a home directory and restricting directory access are foundational to user isolation and least privilege. `sudo ls -ld` checks directory metadata; it does not test Bob's password, interactive login, or access to files inside the directory.

**Next verification (not yet shown as executed):**

```bash
id bob
getent passwd bob
sudo stat /home/bob
```

**Evidence:** LabEx terminal showing `bob` account creation and home-directory verification.

![LabEx terminal showing bob creation with a home directory and verified ownership and permissions](screenshots/labex-user-creation-bob-home-directory-permissions.png)

*Figure 2: Creating `bob` with `useradd -m` and verifying `/home/bob` ownership and mode `750`.*

## Password configuration

```bash
sudo passwd joker
```

The terminal reports `passwd: password updated successfully`, confirming a successful password update. Password input is not echoed in the terminal. Successful interactive login is evidenced in the sudo-group verification section below. The lab's example password is for practice only; use a strong, unique password for real accounts.

![Successful password update for joker in the LabEx terminal](screenshots/labex-joker-password-configuration.png)

*Figure 3: Successful password configuration for `joker`.*

## Modifying the configured home path

```bash
sudo usermod -d /home/wayne joker
sudo grep -w joker /etc/passwd
```

Observed account record:

```text
joker:x:5001:5001::/home/wayne:/bin/sh
```

The `-d` option changes the account's configured home path. The returned record confirms `/home/wayne`. This command does not create the directory or move its contents; those operations are not evidenced here. UID and primary GID remain `5001`.

![Terminal verification of joker's configured home path as /home/wayne](screenshots/labex-joker-home-path-verification.png)

*Figure 4: Home-path modification verified in the account database.*

[LabEx user-properties instructions](screenshots/labex-user-properties-instructions.png) provide exercise context only.

## Changing the login shell

```bash
sudo usermod -s /bin/bash joker
sudo grep -w joker /etc/passwd
```

Observed account record:

```text
joker:x:5001:5001::/home/wayne:/bin/bash
```

The `-s` option changes the configured login shell. The returned entry confirms `/bin/bash`; the home path, UID, and primary GID remain unchanged. This screenshot verifies account configuration. A subsequent login session is evidenced below.

![Terminal verification of joker's login shell as /bin/bash](screenshots/labex-joker-bash-shell-verification.png)

*Figure 5: Bash login-shell configuration verified in the account database.*

[LabEx shell instructions](screenshots/labex-user-shell-instructions.png) provide exercise context only.

## Supplementary group membership and privileged access

```bash
sudo usermod -aG sudo joker
groups joker
su - joker
sudo cat /etc/shadow
exit
```

The group query returns:

```text
joker : joker sudo
```

- `-G sudo` selects a supplementary group; `-a` appends it without replacing existing supplementary memberships.
- `groups joker` confirms membership in `joker` and `sudo`.
- `su - joker` starts a login session as `joker`. The changed prompt confirms the user switch.
- The privileged read returns protected account data after the sudo password prompt, demonstrating successful sudo execution under the lab's policy.
- The subsequent screenshot shows `exit` and return to the `labex` prompt.

![LabEx terminal confirming sudo-group membership, login as joker, and privileged command execution](screenshots/labex-joker-sudo-group-login-verification.png)

*Figure 6: Group membership, authenticated user switching, and sudo execution. This image contains no password hashes.*

[LabEx group-management instructions](screenshots/labex-sudo-group-instructions.png) provide exercise context.

**Security interpretation:** Sudo authorization depends on sudoers policy; a group name alone does not universally grant administrative rights. Adding a user to an administrative group is a significant privilege change and should follow least privilege.

**Evidence handling:** The screenshot containing password hashes and the instruction image displaying the lab password are excluded from the public repository. Protected authentication data is not reproduced in this report. For future verification, use `sudo -l` to inspect allowed commands and `sudo id` to demonstrate elevated execution without exposing password records.

## Next steps

Continue the LabEx user-management exercise, then document account locking and safe account deletion as actually performed. Verify the configured home directory exists and inspect sudo policy with non-sensitive commands.

[← Linux portfolio index](../../README.md)
