---
name: linux-basics-for-hackers
description: Teach and apply Linux command-line fundamentals from a security perspective — filesystem navigation, text filtering, networking, packages, permissions, processes, environment variables, bash scripting, logging, services, anonymity, wireless, kernel modules, scheduling and Python basics. Use when someone is learning Linux for cybersecurity or Kali, asks what a Linux command does, needs a beginner-friendly ("explain like I'm 5") explanation of a Linux concept, or wants a safe practice exercise.
---

# Linux Basics for Hackers — Skill

Knowledge distilled (in original words) from the concepts covered in *Linux Basics for Hackers* by OccupyTheWeb (No Starch Press). Not affiliated with the author or publisher. No book text is reproduced; read the book for full depth.

## How to teach with this skill

1. **ELI5 first** — one everyday analogy (house, keys, diary, CCTV…). Max 2 sentences.
2. **Then the real thing** — the exact command, what each part means.
3. **Security angle** — why an attacker *and* a defender care.
4. **Safe practice** — a one-line exercise to run in a VM (Kali/Ubuntu), never on systems you don't own.
5. If the learner writes in Arabic, answer in Arabic and keep commands and technical terms in English.

## Core concepts (by chapter theme)

| # | Theme | ELI5 analogy | Key commands | Security angle |
|---|---|---|---|---|
| 1 | Basics & filesystem | Linux is a house; `/` is the front door, folders are rooms | `pwd`, `whoami`, `cd`, `ls -la`, `man`, `locate`, `which`, `find`, `grep`, `touch`, `mkdir`, `cp`, `mv`, `rm` | Know where you are and who you are before acting; `whoami` tells you if you're root |
| 2 | Text manipulation | Reading a long letter: first lines, last lines, highlighter | `cat`, `head`, `tail`, `nl`, `grep`, `sed`, `less` | Config files and logs are text — filtering them fast is a core skill |
| 3 | Networking | Your computer's home address and phone book | `ip a` / `ifconfig`, `iwconfig`, `dhclient`, `dig`, `/etc/resolv.conf`, `/etc/hosts` | MAC/IP changes and DNS tampering are both attack and privacy techniques |
| 4 | Software | An app store with trusted shelves | `apt search/install/remove/update/upgrade`, `/etc/apt/sources.list`, `git clone` | Only add trusted repositories — a bad source = installing malware |
| 5 | Permissions | Who holds which key: read 👀, write ✏️, run ▶️ | `ls -l`, `chown`, `chgrp`, `chmod 755`, `chmod u+x`, `umask` | SUID/SGID files run with the owner's power — a classic privilege-escalation path: `find / -perm -4000 2>/dev/null` |
| 6 | Processes | Every running app is a worker with an ID badge | `ps aux`, `top`, `nice`/`renice`, `kill -9 PID`, `&`, `fg`, `at` | Spotting strange processes is how you notice malware |
| 7 | Environment variables | Sticky notes the shell reads before every job | `env`, `set`, `echo $PATH`, `export`, `PS1` | A tampered `PATH` can make you run a fake command |
| 8 | Bash scripting | A recipe the computer follows step by step | `#!/bin/bash`, `chmod +x`, variables, `read`, loops | Automate repetitive checks (e.g., a simple port-scan wrapper in a lab) |
| 9 | Compression & archiving | Vacuum-packing clothes into one bag | `tar -cvf/-xvf`, `gzip`, `bzip2`, `dd` | `dd` makes bit-for-bit copies — used in forensics |
| 10 | Storage devices | Drives are shelves you must mount before using | `lsblk`, `mount`, `umount`, `df -h`, `fsck` | Mounting evidence read-only protects it |
| 11 | Logging | The house's CCTV recordings | `/var/log/`, `rsyslog`, `logrotate`, `journalctl` | Attackers try to erase logs; defenders protect and review them |
| 12 | Services | Shops in the house that open for visitors | `systemctl start/stop/status`, Apache, OpenSSH, MySQL | Every open service is a door — close what you don't need |
| 13 | Security & anonymity | Wearing a hoodie in public, or using a trusted tunnel | Tor, proxychains, VPNs, encrypted email | Understand what each tool hides — and what it doesn't |
| 14 | Wireless | Radios shouting to everyone nearby | `iwconfig`, `iwlist`, `nmcli`, `hciconfig`, `hcitool` | Wi-Fi and Bluetooth leak information to anyone listening |
| 15 | Kernel & modules | The building's foundation; modules are add-on floors | `uname -a`, `sysctl`, `lsmod`, `modprobe` | Malicious kernel modules (rootkits) are the deepest hiding place |
| 16 | Scheduling | An alarm clock for tasks | `crontab -e`, cron syntax `m h dom mon dow`, `rc` scripts | Unknown cron jobs are a common persistence trick — check them |
| 17 | Python basics | A more powerful recipe language | variables, lists, functions, `socket`, `import` | Most security tools are Python; reading them is a superpower |

## Defensive quick checks (safe to run on your own machine)

```bash
whoami && id                         # who am I, which groups
ls -la ~                             # hidden files in home
ps aux --sort=-%cpu | head           # busiest processes
sudo ss -tulpn                       # what is listening on the network
crontab -l; ls /etc/cron.*           # scheduled tasks
find / -perm -4000 -type f 2>/dev/null   # SUID binaries
sudo tail -n 50 /var/log/auth.log    # recent logins (Debian/Ubuntu/Kali)
```

## Guardrails

- Practice only in your own VM or legal labs (TryHackMe, HackTheBox, OverTheWire "Bandit").
- Never run `rm -rf`, `dd`, or `chmod -R 777` on a real system without being sure of the target.
- For anonymity tools, explain limits honestly — no tool makes illegal activity safe.

## Learning path

Next skills in this series: `practical-packet-analysis` → `penetration-testing-weidman` → `metasploit-pentesters-guide`.
