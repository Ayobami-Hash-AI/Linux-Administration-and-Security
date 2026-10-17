# Lab 004 — Application Log Filtering and Error Reporting

**Platform:** LabEx  
**Date:** October 17, 2026  
**Status:** Filtering command evidenced; report contents and match count not yet inspected.

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

[← Linux portfolio index](../../README.md)
