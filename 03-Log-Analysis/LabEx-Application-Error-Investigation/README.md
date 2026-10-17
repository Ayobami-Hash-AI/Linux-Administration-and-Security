# Lab 004 — Application and Kernel Log Investigation

**Platform:** LabEx  
**Date:** October 17, 2026  
**Status:** Application filtering command evidenced; kernel report contents verified. Application report contents remain uninspected.

## Objective

Find lines containing `ERROR` in `~/project/logs/app.log` and save them to `~/project/error_report.txt`.

## Actions performed

I inspected the project directory, located `app.log` inside `logs`, returned to the project directory, and used `grep` with output redirection to create an error report.

```bash
ls
cd /logs/app.log
# cd: no such file or directory: /logs/app.log
cd logs
ls
cd ..
grep "ERROR" ~/project/logs/app.log > ~/project/error_report.txt
```

## Command interpretation

| Component | Purpose |
|---|---|
| `grep "ERROR"` | Select lines containing the case-sensitive text `ERROR` |
| `~/project/logs/app.log` | Input log path under the current user's home directory |
| `>` | Redirect standard output into a file, creating it or overwriting existing contents |
| `~/project/error_report.txt` | Destination report |

The search matches `ERROR` anywhere in a line; it does not enforce whole-word boundaries. No terminal output is expected for matching lines because standard output is redirected.

## Troubleshooting

My initial command used `/logs/app.log`, an absolute path starting at the filesystem root. The intended location is under `~/project/logs/`. Also, `cd` changes into directories, not log files. I corrected the navigation with `cd logs` and used `grep` to read the file.

## Evidence

![LabEx task to extract ERROR lines into error_report.txt](screenshots/labex-application-error-filtering-task.png)

*Figure 1: Application-log filtering requirement.*

![Terminal showing path troubleshooting and grep output redirection](screenshots/labex-grep-error-report-command.png)

*Figure 2: Corrected navigation and error-report command.*

The screenshot shows the filtering command returning to the prompt without a visible error. It does not show the report contents, number of matches, or command exit status. An empty report remains possible if no lines match.

## Verification to perform next

```bash
cat ~/project/error_report.txt
wc -l ~/project/error_report.txt
```

These checks are suggested, not yet evidenced. Review report contents before publishing logs that might contain sensitive information.

## Security relevance

Filtering logs reduces investigation noise and helps isolate application failures. An `ERROR` label alone does not establish malicious activity; assess timestamps, surrounding events, affected services, and other telemetry.

## Investigating kernel boot messages

I filtered the kernel ring buffer for case-insensitive matches to `fail` or `error`, saved the results, and displayed the report.

```bash
sudo dmesg | grep -i -E 'fail|error' > ~/project/boot_issues.txt
cat ~/project/boot_issues.txt
```

| Component | Meaning |
|---|---|
| `sudo dmesg` | Read kernel ring-buffer messages with elevated privileges |
| `\|` | Send the first command's standard output to the next command |
| `grep -i` | Match without case sensitivity |
| `-E 'fail\|error'` | Use extended regular expressions; match either alternative |
| `>` | Save matching lines to the report, overwriting any prior contents |
| `cat` | Display the saved report |

Three lines are visible in the report:

1. An ACPI message states that MMCFG information could not be added and extended PCI configuration space could not be accessed under a bridge.
2. `RAS: Correctable Errors collector initialized.`
3. A `my-driver` probe for `0000:00:1f.0` failed with `error -2`.

**Interpretation:** The RAS initialization line is a keyword match, not evidence by itself of a current failure. The ACPI and driver messages warrant investigation, but the screenshot does not establish their cause, severity, or connection to the application errors. Kernel output can include events after boot as well as boot-time messages.

![LabEx task to search kernel messages for fail or error](screenshots/labex-kernel-boot-investigation-task.png)

*Figure 3: Kernel-message investigation requirement.*

![Terminal showing dmesg filtering and three saved report lines](screenshots/labex-dmesg-boot-issues-report-verification.png)

*Figure 4: Saved kernel report inspected using cat.*

**Investigation lesson:** Keyword filtering produces candidates for review. Read surrounding messages and correlate symptoms before concluding that an application issue is caused by a kernel or hardware problem. No remediation or confirmed root cause is evidenced here.

## Examining Nginx configuration and appending evidence

I searched the lab's Nginx configuration for `worker_processes` and redirected the matching lines to the end of the existing error report.

```bash
sudo grep "worker_processes" ~/project/config/nginx.conf >> ~/project/error_report.txt
```

**Command interpretation:** `grep` selects lines containing the text `worker_processes`. The `>>` operator appends output, preserving existing report contents; `>` would overwrite them. The shell performs the redirection as the current user even though `grep` runs under `sudo`.

The terminal returns to the prompt without a visible error. Because output is redirected and the report is not displayed, the screenshot does not establish whether a line matched, the directive's configured value, or what was appended. A simple text search can also match commented lines.

![LabEx task to locate worker_processes and append the line to the report](screenshots/labex-nginx-worker-processes-task.png)

*Figure 5: Configuration-review and report-append requirement.*

![Terminal showing grep for worker_processes with append redirection](screenshots/labex-nginx-directive-report-append-command.png)

*Figure 6: Nginx configuration search and append command.*

**Verification to perform next (not yet evidenced):**

```bash
grep "worker_processes" ~/project/config/nginx.conf
tail -n 10 ~/project/error_report.txt
```

**Investigation limits:** The task narrative suggests investigating worker-process configuration. The screenshots do not establish that too few workers caused a bottleneck, that hardware faults have been ruled out, or that this lab configuration is the running server's active configuration. No configuration change, reload, or performance test has been demonstrated.

[← Linux portfolio index](../../README.md)
