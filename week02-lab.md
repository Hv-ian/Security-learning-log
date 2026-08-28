# Week 2 — Lab: Linux fundamentals

## What I did

* Navigated and enumerated a Linux system using commands such as `pwd`, `whoami`, `id`, `hostname`, `ls`, and `uname`.
* Inspected `/etc/passwd` to understand system accounts, user IDs, and service accounts.
* Practiced Linux file permissions by creating a file and changing its permissions with `chmod 600`, `644`, `777`, and `000`.
* Hunted for SUID binaries with `find / -perm -4000 -type f` and reviewed world-writable files under `/etc`.
* Investigated authentication activity in `/var/log/auth.log`, including successful `sudo` activity and failed login attempts.
* Created and monitored a cron job that wrote timestamps to a log file every minute, then removed it after testing.
* Generated an Ed25519 SSH key pair, copied the public key to `ubuntu-srv`, and verified passwordless key-based authentication.
* Hardened SSH by disabling password authentication on `ubuntu-srv`.

## The command that made something click

* `find / -perm -4000 -type f 2>/dev/null` — this made SUID permissions click for me because it showed how a defender can systematically search the entire system for programs with elevated execution privileges instead of checking files manually.

## What broke and how I fixed it

* The lab network configuration reset after reboot, so I restored the `192.168.56.10/24` and `192.168.56.11/24` addresses and verified connectivity with `ping`.
* I intentionally changed `report.txt` to `chmod 000`, which resulted in `Permission denied`. I restored access with `chmod 600`.
* I created a cron job for testing and later removed it to avoid leaving unnecessary scheduled tasks behind.
* SSH initially required a password. I generated an Ed25519 key, installed the public key with `ssh-copy-id`, and verified key-based login before disabling password authentication.

## The hardening I performed

* Disabled SSH password login on `ubuntu-srv` by setting `PasswordAuthentication no` in `/etc/ssh/sshd_config`.
* Restarted the SSH service and verified that key-based authentication still worked without a password.
* Why it matters: disabling password authentication removes password guessing and brute-force attacks as an SSH authentication path. An attacker can no longer simply keep trying passwords against the account; they need a valid authentication key instead.
* Took a VM snapshot named `week2-ssh-hardened` so the hardened configuration could be restored if needed.
