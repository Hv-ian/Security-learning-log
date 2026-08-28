# Week 1 — Lab: Building the stand

## What I built

* Set up the cybersecurity lab environment in VirtualBox with four isolated virtual machines:

  * `ubuntu-lab` — Ubuntu Desktop, 4 GB RAM, 2 CPUs, 25 GB disk
  * `ubuntu-srv` — Ubuntu Server, 1 GB RAM, 1 CPU, 15 GB disk, OpenSSH installed
  * `kali` — Kali Linux, 2 GB RAM, 2 CPUs, 30 GB disk
  * `win10-lab` — Windows 10, 4 GB RAM, 2 CPUs, 40 GB disk
* Configured an isolated VirtualBox Internal Network named `labnet` so the VMs could communicate with each other without connecting to the internet or the wider network.
* Assigned manual IP addresses to the lab machines:

  * `ubuntu-lab` — `192.168.56.10`
  * `ubuntu-srv` — `192.168.56.11`
  * `kali` — `192.168.56.20`
  * `win10-lab` — `192.168.56.30`
* Tested connectivity between Kali and both Ubuntu machines using `ping`.
* Observed that Windows did not respond to ICMP ping because Windows Firewall blocks it by default.
* Created named VirtualBox snapshots for all four machines and tested restoring the Ubuntu Desktop snapshot.
* Started the learning log repository and created the first weekly entry.

## What broke and how I fixed it

* The lab required careful RAM management because the host only has 16 GB. I learned to avoid running all four VMs simultaneously and instead run only the machines needed for each task.
* The VMs initially had no communication because the Internal Network needed to be configured consistently across the machines.
* The isolated network had no DHCP, so I manually assigned the `192.168.56.x` addresses to each VM.
* Windows did not respond to ping from Kali. This was not a network failure — Windows Firewall was correctly dropping ICMP traffic.
* I learned that the manual IP configuration resets after reboot, which is expected for this stage of the lab. Permanent network configuration comes later.

## What I did not understand yet

* I initially needed a better understanding of how VirtualBox networking modes differ, especially why an Internal Network provides VM-to-VM communication but no internet access.
* I also needed to understand the difference between a VM snapshot and a normal backup. The snapshot exercise made it clear that a snapshot captures a VM's state so I can quickly roll back after experimenting or breaking something.

## Network map

* `ubuntu-lab`: `192.168.56.10` — Ubuntu Desktop
* `ubuntu-srv`: `192.168.56.11` — Ubuntu Server, no GUI, SSH installed
* `kali`: `192.168.56.20` — Kali Linux
* `win10-lab`: `192.168.56.30` — Windows 10, firewall drops ICMP

## The command that made something click

* `ping -c 4 192.168.56.11` — this was the first moment the isolated lab felt like a real network. Seeing Kali successfully communicate with the Ubuntu machines confirmed that the VMs were connected to the same private network while remaining isolated from everything outside the lab.

## The security lesson that stuck

* **Authorization comes before technical ability.** The lab is deliberately isolated so I can practice security tools against systems I am authorized to test. Having tools such as Nmap or Wireshark is not what makes an activity legitimate — permission and scope do.
* The Windows Firewall also provided an early example of a security control working as intended: the machine was reachable on the lab network but did not respond to ICMP requests.
