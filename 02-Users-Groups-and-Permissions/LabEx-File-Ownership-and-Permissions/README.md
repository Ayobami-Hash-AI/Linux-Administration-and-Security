# Lab 001 — Linux File Ownership and Permissions

**Platform:** LabEx | **Date documented:** October 9, 2026  
**Topic:** Linux administration, discretionary access control, least privilege  
**Status:** Initial hands-on exercise documented from a shared terminal screenshot

## Objective
Create a file, assign its user and group ownership, configure permissions using octal notation, and verify the result.

## Commands observed

```bash
touch target_file
sudo chown user1:group1 target_file
sudo chmod 760 target_file
ls -l target_file
```

The screenshot shows a permissions listing consistent with:

```text
-rwxrw---- 1 user1 group1 ... target_file
```

## Step-by-step explanation

1. **`touch target_file`** creates an empty file if it does not already exist.
2. **`sudo chown user1:group1 target_file`** changes the file owner to `user1` and group to `group1`. Elevated privileges are commonly required.
3. **`sudo chmod 760 target_file`** sets permissions for the owner, group, and everyone else.
4. **`ls -l target_file`** displays the resulting permissions and ownership.

| Subject | Octal | Symbolic | Access |
|---|---:|---|---|
| Owner (`user1`) | 7 | `rwx` | Read, write, execute |
| Group (`group1`) | 6 | `rw-` | Read, write |
| Others | 0 | `---` | No access |

**Interpretation:** The leading `-` denotes a regular file. The next nine characters express owner, group, and other permissions. Execution permission on a file does not mean its contents are a valid executable or script.

## Security relevance

Linux ownership and permissions are basic controls supporting **least privilege**. Overly permissive files may enable unauthorized modification or disclosure. For example, `777` allows all local users read/write/execute access under traditional mode bits, which may be inappropriate for sensitive files. The correct permission setting depends on the file's purpose and access requirements.

### Healthcare security connection

On systems supporting clinical services, carefully scoped access to application scripts, configurations, and sensitive files helps reduce the risk of unauthorized changes and disruptions. Permissions are one layer of protection; access-control lists, service identities, auditing, and application-level controls also matter.

## Evidence and limitations

- **Evidence reviewed:** A LabEx terminal screenshot shared during the learning session.
- **Observed outcome:** Ownership `user1:group1` and mode `760` on `target_file`.
- **Screenshot publication:** The screenshot is not yet uploaded to this repository. Add a sanitized image to an `evidence/` directory when available.
- No additional commands, troubleshooting steps, or outcomes are claimed beyond the shared evidence.

## Next practice

- Compare `chmod 600`, `640`, `700`, `750`, and `755` on appropriate test files.
- Explore `id`, `groups`, `stat`, `umask`, and directory permissions.
- Study SUID, SGID, and the sticky bit in a controlled lab.
- Investigate how Linux permissions interact with secure service configuration.

---
*All exercises should be performed in authorized lab environments.*
