# Linux Admin Ticket Queue

[Claude Context File Here](https://github.com/traceadugar/ProjectJournal/blob/main/April%2026/context_file/context.md)

## Overview

The Linux Admin Ticket Queue is a stateful Linux practice lab designed to turn Linux+ study into realistic operator reps.

Instead of memorizing commands in isolation, this project simulates the way Linux problems actually appear in a NOC, helpdesk, junior sysadmin, or cloud operations environment: as tickets with symptoms, incomplete context, risk, and a need for careful investigation.

The system uses a real Linux server or VM as the foundation, then layers a simulated practice environment on top of it. Claude Code acts as the simulation engine and learning mentor, issuing one realistic Linux admin ticket at a time through a reusable `/linux-ticket` skill.

Each ticket requires me to inspect before acting, explain my reasoning, apply the narrowest correct fix, verify the result, and write a professional resolution note.

The core rule of the project is simple:

> No answer is given before inspection. No shortcuts. No `chmod 777`. Just real Linux work.

---

## Why I Built This

While studying for CompTIA Linux+, I realized that memorizing commands is not enough.

Linux administration requires judgment. It is not just knowing that `chmod`, `chown`, `systemctl`, `journalctl`, `dnf`, `ip`, or `sshd_config` exist. It is knowing when to use them, what to check first, what not to change, how to avoid over-permissioning, and how to verify that the fix actually worked.

I built this project because I wanted a repeatable way to practice Linux administration through realistic reps.

The goal is to build durable operator instincts:

- inspect before changing
- form a hypothesis
- verify assumptions
- apply least privilege
- avoid risky shortcuts
- document the resolution
- track weak areas
- improve through repetition

This project turns certification study into a living practice environment.

---

## What This Project Simulates

The Linux Admin Ticket Queue simulates a small Linux operations environment.

The lab begins with real system information gathered from a Linux server or VM. That baseline includes users, groups, services, disk layout, network configuration, SSH settings, cron jobs, firewall status, and installed packages.

From there, Claude Code builds a simulated practice layer on top of the real machine. This layer can include fictional users, groups, shared directories, service accounts, misconfigurations, broken permissions, service failures, logging issues, package problems, SSH access issues, DNS problems, automation errors, and basic hardening tasks.

The result is a stateful Linux lab where the environment changes over time.

Future tickets are based on:

- the current environment state
- previous tickets
- completed fixes
- unresolved issues
- weak areas
- commands already practiced

This prevents the practice from feeling random. The server evolves as I work through the queue.

---

## How It Works

```text
I provide Claude Code with real Linux system information
        ↓
Claude creates an initial environment state file
        ↓
Claude adds a simulated practice layer on top of the real system
        ↓
The /linux-ticket skill reads the current state files
        ↓
Claude issues one realistic admin ticket
        ↓
I inspect, diagnose, fix, verify, and write a resolution note
        ↓
Claude reviews my work and updates the environment state
        ↓
The next ticket targets either a new Linux+ topic or a weak area
```

The project uses three persistent files:

| File | Purpose |
|---|---|
| `environment_state.md` | Living server state updated after every ticket |
| `ticket_log.md` | Completed ticket history |
| `weak_areas.md` | Running gap tracker used to target future reps |

These files act as the memory of the project.

---

## Claude Code Skill

To make the system reusable across sessions, I created a Claude Code skill called:

```text
/linux-ticket
```

The skill lives at:

```text
~/.claude/skills/linux-ticket/SKILL.md
```

The skill reads the three state files before issuing a ticket:

```text
environment_state.md
weak_areas.md
ticket_log.md
```

This allows the ticket queue to continue without re-pasting context every time.

The skill is responsible for:

- reading the current environment state
- checking completed tickets
- reviewing weak areas
- determining the next appropriate tier
- issuing exactly one ticket
- withholding the answer until inspection is performed
- enforcing least privilege and safe troubleshooting habits

This was an important part of the project because the first version of the skill did not work as expected. The fix was learning that Claude Code skills need to be placed inside their own named subdirectory with a `SKILL.md` file, rather than being saved as a flat Markdown file directly in the skills folder.

That troubleshooting became part of the project itself: the simulator needed proper structure before it could behave like a repeatable tool.

---

## Skills Practiced

This lab is designed around Linux+ and real-world Linux operations.

### Users, Groups, and Permissions

Commands and concepts:

- `id`
- `groups`
- `getent passwd`
- `getent group`
- `useradd`
- `usermod`
- `passwd`
- `groupadd`
- `chown`
- `chmod`
- `sudo -l`
- `visudo`
- setgid directory behavior
- least privilege
- permission inheritance
- avoiding `chmod 777`

### Processes, Services, and Logs

Commands and concepts:

- `ps`
- `grep`
- `pgrep`
- `kill`
- `systemctl status`
- `systemctl restart`
- `systemctl enable`
- `journalctl`
- service failure diagnosis

### Packages, Repos, and Updates

Commands and concepts:

- `dnf`
- `apt`
- `rpm`
- `dpkg`
- repository checks
- cache issues
- failed updates
- package verification

### Networking and DNS

Commands and concepts:

- `ip a`
- `ip route`
- `ping`
- `curl`
- `ss`
- DNS troubleshooting
- firewall reasoning
- service exposure

### Storage and Mounts

Commands and concepts:

- `df -h`
- `du -sh`
- `lsblk`
- `/etc/fstab`
- mount troubleshooting
- disk usage investigation
- safe cleanup

### SSH and Remote Access

Commands and concepts:

- SSH login failures
- key permissions
- `sshd_config`
- root login policy
- deprecated algorithms
- safe remote access troubleshooting

### Automation

Commands and concepts:

- cron
- systemd timers
- Bash scripting
- logs
- exit codes
- safe automation habits

### Security Hardening

Commands and concepts:

- least privilege
- sudo restrictions
- file exposure
- service exposure
- audit-style thinking
- SELinux basics
- avoiding risky shortcuts

---

## Ticket Workflow

Every ticket follows the same operational workflow:

```text
1. Ticket issued
2. Read-only inspection
3. Hypothesis
4. Diagnosis
5. Fix
6. Verification
7. Resolution note
8. Review
9. Environment update
10. Weak-area update
```

The simulator does not give away the answer immediately.

Before making a change, I have to explain what I am checking and why. This forces me to practice the thought process behind the command, not just the syntax.

For example, instead of only saying:

```bash
id jordan
```

The expected reasoning is:

```text
I would run `id jordan` to verify whether the user is actually a member of the group that should have access before changing permissions.
```

That distinction is the point of the project.

---

## Safety Rules

The simulator enforces safety rules during every ticket:

- run inspection commands before making changes
- explain the reason for each command
- avoid `chmod 777`
- avoid over-permissioned fixes
- prefer group-based access over one-off permission hacks
- use least privilege
- verify before and after changes
- take snapshots before higher-risk tiers
- simulate dangerous changes when appropriate
- write a professional resolution note before closing the ticket

The project also includes a privacy warning before pasting system output into an AI tool. Public IPs, private hostnames, internal domain names, SSH banners, API keys, tokens, and production details should be redacted.

---

## Difficulty Tiers

The ticket queue progresses through nine tiers.

| Tier | Focus Area |
|---|---|
| 1 | Users, Groups, Permissions |
| 2 | Processes, Services, Logs |
| 3 | Packages, Repos, Updates |
| 4 | Networking and DNS |
| 5 | Storage, Mounts, Disk Usage |
| 6 | SSH and Remote Access |
| 7 | Automation |
| 8 | Security Hardening |
| 9 | Mixed Incidents |

The lab starts at Tier 1 regardless of experience level.

The point is not speed. The point is depth, judgment, and repeatable troubleshooting.

---

## Example Ticket Format

Each ticket is issued like a real support request.

```text
Ticket: LAQ-001
Priority: Medium
Category: Users, Groups, Permissions
Reporter: Operations Team Lead

User jordan reports that they can see the shared reports directory but cannot create files inside it.

Path:
/srv/ops/reports

Error:
touch: cannot touch '/srv/ops/reports/test.txt': Permission denied

Members of the ops group should be able to create and edit files in this directory.

Do not use chmod 777.
Inspect before making changes.
```

The ticket gives a symptom, not the answer.

---

## Example Resolution Workflow

A clean resolution would involve:

1. verifying the user exists
2. checking the user’s group membership
3. inspecting the directory ownership and permissions
4. identifying the mismatch
5. applying the narrowest correct fix
6. verifying access
7. documenting the result

Example inspection commands might include:

```bash
id jordan
getent group ops
ls -ld /srv/ops /srv/ops/reports
namei -l /srv/ops/reports
```

A correct fix might involve group ownership, group write permission, and possibly the setgid bit so newly created files inherit the correct group.

The review would then update:

- `environment_state.md`
- `ticket_log.md`
- `weak_areas.md`

---

## Environment State Tracking

The environment state file is the source of truth for the simulator.

It tracks:

- real machine facts
- simulated lab resources
- users
- groups
- directories
- permissions
- services
- packages
- network settings
- SSH policies
- cron jobs
- known issues
- resolved changes
- weak areas
- future ticket seeds

A key design rule is separating real system state from simulated lab state.

Resources should be marked clearly as:

```text
[REAL]
[SIMULATED]
[PLANNED]
[UNKNOWN]
```

This prevents confusion between the actual server and the practice layer.

---

## Linux+ Exam Alignment

This project maps directly to CompTIA Linux+ XK0-006 objectives.

The simulator reinforces:

- system management
- security
- scripting and automation
- troubleshooting
- Linux operations and deployment

The ticket format is especially useful for performance-based questions because those require practical troubleshooting, not just recognition of terms.

Linux+ is not only about knowing commands. It is about working through a system, identifying the problem, applying the right fix, and verifying the result.

That is exactly what this project trains.

---

## Cloud and SRE Relevance

Although this project is Linux-focused, the workflow transfers directly to cloud operations, cloud security, and SRE-style work.

The skills map cleanly:

| Linux Practice | Cloud/SRE Relevance |
|---|---|
| Service failure diagnosis | Cloud VM and workload troubleshooting |
| Users, groups, and sudo | IAM and least privilege |
| Logs and `journalctl` | Cloud logging and observability |
| Firewall and port checks | Security groups, VPC rules, and ingress control |
| Bash automation | Operational scripting and toil reduction |
| SSH hardening | Secure remote access patterns |
| Disk and mount issues | Persistent disk and storage troubleshooting |
| Resolution notes | Incident documentation and ticket closure |

The methodology is the real value:

```text
inspect → hypothesize → change carefully → verify → document
```

That process applies whether the system is a homelab server, a cloud VM, a Kubernetes node, or a production incident.

---

## Lessons Learned

This project reinforced several important lessons.

### 1. Repetition Needs Context

Practicing random commands is useful, but realistic tickets make the commands stick better.

A command becomes easier to remember when it is attached to a real problem.

### 2. State Matters

A realistic environment should remember what changed.

If every ticket is isolated, the practice feels artificial. By tracking environment state, future tickets can build on previous work.

### 3. Least Privilege Has to Be Practiced

It is easy to know that `chmod 777` is bad. It is more valuable to practice finding the correct narrow fix under pressure.

### 4. Good Troubleshooting Is Structured

The best habit is not guessing the fix. It is inspecting first, forming a hypothesis, and verifying the result.

### 5. Tooling Matters

The `/linux-ticket` skill reduced friction. Once the skill worked correctly, the simulator became easier to reuse across sessions.

### 6. Documentation Is Part of the Skill

The resolution note is not extra. It is part of the work. A good Linux admin should be able to explain what changed and why.

---

## Future Improvements

Future improvements for this project include:

- adding Git tracking for environment state changes
- creating sample completed tickets
- adding more advanced mixed-incident scenarios
- building a scoring rubric
- adding a command comfort rating system
- creating separate RHEL and Ubuntu tracks
- adding SELinux-specific tickets
- adding cloud VM scenarios
- adding Terraform-based lab deployment
- creating a disposable VM image for repeatable resets
- turning common weak areas into targeted drills

---

## Final Reflection

The Linux Admin Ticket Queue is a deliberate-practice system for building Linux operator skill.

It takes the pressure I felt around forgetting command syntax and turns it into a repeatable training loop. Instead of relying only on passive memorization, I can work through realistic tickets, make mistakes safely, document what I learn, and build confidence through reps.

This project also fits my larger career direction. I am working toward cloud security, automation, and infrastructure roles, and Linux is one of the foundations underneath that path.

The real outcome of this project is not just a stronger Linux+ study routine.

The outcome is better troubleshooting judgment.

That is the skill I am actually building.
