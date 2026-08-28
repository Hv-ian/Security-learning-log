# Week 3 — Lab: Windows fundamentals

## What I did

* Used PowerShell to enumerate local users, groups, services, processes, and SMB shares.
* Checked membership of the `Administrators` group to understand Windows privileges and identify accounts with administrative access.
* Created a test account called `backup_svc` and added it to the `Administrators` group to simulate an attacker establishing privileged access.
* Generated failed and successful logons, then investigated the resulting events in Windows Event Viewer.
* Learned to identify Event IDs `4720` (account created), `4732` (added to a privileged group), `4625` (failed logon), and `4624` (successful logon).
* Investigated Logon Type values and connected Type 2 with interactive keyboard logins.
* Created and deleted a harmless Registry value under `HKEY_CURRENT_USER\Control Panel\Desktop` to practice navigating the Windows Registry safely.
* Built a PowerShell investigation script, `hunt.ps1`, that lists Administrators, searches for failed logon events, and counts the results.
* Took a `week3-windows-explored` VM snapshot after completing the lab.

## The attack I ran on myself, and how I found it

* Created the `backup_svc` account with `net user ... /add`, generating Event ID `4720`.
* Added `backup_svc` to the `Administrators` group, generating Event ID `4732`.
* Intentionally entered an incorrect password several times, generating Event ID `4625`.
* Logged in successfully, generating Event ID `4624`.
* Used Event Viewer → Windows Logs → Security to filter for these Event IDs and trace the activity.
* The key lesson was that actions which look like an attacker gaining persistence or privileges leave useful evidence in the Windows Security log.

## How I built my script

* I searched for the individual PowerShell building blocks instead of copying a complete script blindly.
* I used `Get-LocalGroupMember` to enumerate the Administrators group and `Get-WinEvent` to retrieve Security events.
* I combined the event results with filtering and counting so the script could both show failed logons and report how many were found.
* I made sure I could explain what each line was doing before running `hunt.ps1`.

## The Linux <-> Windows link that stuck

* Linux `/var/log` ↔ Windows Event Viewer: both provide logs that defenders can investigate to reconstruct activity.
* Linux `cron` ↔ Windows Registry Run keys: both can be abused as persistence mechanisms that automatically execute something later.
* Linux `root` ↔ Windows Administrator/SYSTEM: both represent highly privileged access, although Windows separates Administrator and SYSTEM privileges differently.
* Linux `sudo` ↔ Windows UAC: both introduce a privilege boundary that requires elevation before performing certain administrative actions.
