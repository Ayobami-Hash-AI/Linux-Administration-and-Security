# Lab 002 — Creating and Verifying a Linux User Account

**Platform:** LabEx  
**Date:** October 9, 2026  
**Topic:** Linux user administration and account verification  
**Status:** Initial step completed and evidenced; additional account configuration not yet shown.

## Objective

Create a local Linux account named `joker` and verify that the system registered the account.

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

**Important distinction:** A home directory path appearing in `/etc/passwd` does **not** prove that the directory exists. The screenshot also does not demonstrate that a password was set or that an interactive login succeeds.

## Security significance

Local accounts are security principals. Their UIDs, group memberships, authentication configuration, and login shells affect system access and auditability. Good account management supports least privilege and accountable access.

## Verification to perform next

```bash
id joker
getent passwd joker
ls -ld /home/joker
sudo passwd -S joker
```

These are **suggested follow-up commands**, not commands shown as completed in the screenshot. Do not publish password hashes or credentials.

## Evidence

- **Source:** User-shared LabEx screenshot from October 9, 2026 showing the account-creation command and `/etc/passwd` output.
- **Image file:** Screenshot upload to this GitHub repository is pending; the connected GitHub text-file interface does not accept binary images.
- **Limitations:** No evidence yet of home-directory creation, password configuration, group changes, or login testing.


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

**Evidence:** User-shared LabEx screenshot of the `bob` creation and directory listing. The screenshot is available in the conversation but has not been uploaded as a binary file to GitHub through the current connector.

## Next steps

Continue the LabEx user-management exercise, then document group membership, account modification, account locking, and safe account deletion as actually performed.

[← Linux portfolio index](../../README.md)
