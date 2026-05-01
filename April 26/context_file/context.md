<!--
GitHub Topics (add these to your repo Settings → Topics):
linux, linux-administration, linux-learning, sysadmin, homelab, ctf-practice,
claude-code, ai-tutor, terminal, rhel, ubuntu, linux-plus, comptia, devops-learning,
bash, cli-tools, interactive-learning, self-hosted, open-source
-->

# Linux Admin Ticket Queue

A modular Linux practice simulator that uses Claude Code as a simulation engine and learning mentor. You bring a real Linux server. Claude builds a living environment on top of it and issues one realistic admin ticket at a time.

> **No answer is ever given before you inspect. No shortcuts. No `chmod 777`. Just real Linux work.**

---

## What This Is

A scenario-based Linux practice system designed to build operator instincts, not just memorize commands.

Instead of isolated drills, you work through realistic tickets — the same way problems actually arrive in a NOC, a helpdesk queue, or a junior sysadmin role. Each ticket requires you to inspect before acting, explain your reasoning, apply a least-privilege fix, verify the result, and write a professional resolution note.

Claude maintains a living environment state file that updates after every ticket. Future tickets are based on what changed. The server evolves over time.

**Designed for:**
- CompTIA Linux+ (XK0-006) exam prep
- Junior sysadmin / cloud engineering skill building
- Anyone who learns better by doing than by reading

---

## How It Works

```
You give Claude your real system info
       ↓
Claude builds a simulated environment on top of your real server
       ↓
/linux-ticket issues one realistic admin ticket at a time
       ↓
You inspect, diagnose, fix, verify, and write a resolution note
       ↓
Claude grades your approach and updates the environment state
       ↓
Repeat — the server evolves, weak areas get targeted
```

Three files persist your progress across every session:

| File | Purpose |
|---|---|
| `environment_state.md` | Living server state — updated after every ticket |
| `ticket_log.md` | Completed ticket history |
| `weak_areas.md` | Running gap tracker — future tickets target these |

---

## Prerequisites

**You need:**
- A Linux server or VM you can SSH into
- [Claude Code](https://claude.ai/code) installed and authenticated

**Recommended:**
- RHEL, Rocky Linux, AlmaLinux, or Fedora (RHEL-family)
- Ubuntu/Debian works fine — note distro differences where they come up
- A dedicated lab VM so you can make real changes safely
- Snapshots or backups before Tier 5+ tickets

**Not required:**
- Any prior Linux admin experience
- A physical server — a VM on your laptop works fine

---

## Step 1 — Gather Your System Info

SSH into your server and run these commands. Save the output — you will paste it into Claude in Step 3.

All commands are read-only. Nothing changes.

> **Privacy note:** Review all output before pasting into any AI tool. Redact public IPs, private hostnames, internal domain names, SSH banners, API keys, tokens, or anything tied to a real production environment.

```bash
# System
hostname
cat /etc/os-release
uptime

# Users
cat /etc/passwd | grep -v nologin | grep -v false
cat /etc/passwd
cat /etc/group
getent group wheel sudo
id $USER
ls -la /home/

# Sudo access (requires sudo)
sudo cat /etc/sudoers | grep -v "^#" | grep -v "^$"

# Services
systemctl list-units --type=service --state=active
systemctl list-units --type=service --state=failed

# Disk
df -h
lsblk

# Network
ip a
ip route
cat /etc/resolv.conf
hostname -f

# SSH config
sudo cat /etc/ssh/sshd_config | grep -v "^#" | grep -v "^$"

# Cron
sudo crontab -l
crontab -l

# Firewall — RHEL/Rocky/Alma
sudo firewall-cmd --list-all

# Firewall — Ubuntu/Debian
sudo ufw status verbose

# Installed packages (RHEL family)
rpm -qa | grep -E "nginx|httpd|postfix|audit|fail2ban|chronyd|ntp"

# Installed packages (Debian family)
dpkg -l | grep -E "nginx|apache2|postfix|auditd|fail2ban|chrony|ntp"
```

---

## Step 2 — Create Your Project Directory

On the machine running Claude Code, create a directory to hold your state files:

```bash
mkdir -p ~/linux-ticket-queue
```

You will end up with three files inside it:
- `environment_state.md`
- `weak_areas.md`
- `ticket_log.md`

Note the full path to this directory — you will need it in Step 4.

---

## Step 3 — Prime Claude With Your System

Open Claude Code in your project directory and paste the following prompt. Replace everything in `[ ]` with your actual details.

```
I want to run the Linux Admin Ticket Queue simulator on my Linux server.

Here is my system output from the setup commands:

[PASTE YOUR COMMAND OUTPUT HERE]

Using this real system data, please:

1. Build an initial environment_state.md file that captures my real system state accurately.
2. Design a simulated layer on top of it — fictional users, groups, directories, and misconfigs — that will serve as ticket material.
3. Separate clearly what is REAL (actual machine state) from what is SIMULATED (added for practice).
4. Identify any real observations from my actual machine that are worth turning into hardening tickets later.
5. Do NOT issue Ticket 001 yet. Show me the environment state first for review.

Save the file to: ~/linux-ticket-queue/environment_state.md

Also create these two empty tracking files:
- ~/linux-ticket-queue/weak_areas.md
- ~/linux-ticket-queue/ticket_log.md

My distro is: [RHEL 10 / Ubuntu 24.04 / Rocky 9 / etc.]
My username is: [your username]
My server hostname is: [your hostname]
My experience level: [beginner / some Linux exposure / comfortable with CLI]
```

Review what Claude builds. Correct anything that looks wrong before moving on.

---

## Step 4 — Create the /linux-ticket Skill

This is what makes the simulator work across sessions with a single command.

**Find your Claude Code skills directory:**

| Platform | Path |
|---|---|
| Linux / macOS | `~/.claude/skills/` |
| Windows | `C:\Users\<YourName>\.claude\skills\` |

Create a subdirectory and skill file:

```bash
# Linux/macOS
mkdir -p ~/.claude/skills/linux-ticket
```

```powershell
# Windows (PowerShell)
New-Item -ItemType Directory -Path "$env:USERPROFILE\.claude\skills\linux-ticket" -Force
```

Create the file `SKILL.md` inside that directory with the following content.

**Replace the three file paths with the actual full paths to your state files.**

```markdown
---
name: linux-ticket
description: Issues the next Linux admin ticket for the ticket queue simulator. Reads current environment state, ticket history, and weak areas, then issues one appropriate ticket.
---

Read these three files before doing anything else:
- /home/YOUR_USER/linux-ticket-queue/environment_state.md
- /home/YOUR_USER/linux-ticket-queue/weak_areas.md
- /home/YOUR_USER/linux-ticket-queue/ticket_log.md

Once you have read all three files:

1. Determine the current tier based on the environment state file.
2. Check the ticket log for what has already been completed.
3. Check weak areas for gaps that should be targeted.
4. If setup commands are needed before the ticket (e.g. creating a simulated user or directory), provide them first and wait for confirmation before issuing the ticket.
5. Issue exactly one ticket in the standard format:
   - Ticket number, priority, category, and reporter
   - A realistic symptom description as if submitted by another user
   - No answer or hints — the user must inspect first

Do not issue the review. Do not give hints. Wait for the user to run commands and explain their approach.

Simulator rules that always apply:
- Do not reveal the answer before the user demonstrates inspection
- Require read-only commands before any changes
- No chmod 777 or over-permissioned shortcuts
- Enforce least privilege
- Require verification before and after any fix
- Require a resolution note before giving the review
```

> **Important:** The skill must be a `SKILL.md` file inside a named subdirectory — not a flat `.md` file directly in the `skills/` folder. Claude Code will not load it otherwise.

---

## Step 5 — Start the Queue

Once the skill file exists, type:

```
/linux-ticket
```

Claude will read your three state files and issue the first ticket. No setup prompt, no file paths to remember — just the command.

---

## Continuing Across Sessions

At the start of any future Claude Code session, type:

```
/linux-ticket
```

That is all. The skill reads your current state files and picks up exactly where you left off.

If you have not set up the skill yet, use this fallback prompt instead:

```
I am continuing the Linux Admin Ticket Queue simulator.

Please read these files before we start:
- ~/linux-ticket-queue/environment_state.md
- ~/linux-ticket-queue/weak_areas.md
- ~/linux-ticket-queue/ticket_log.md

Summarize the current environment state and how many tickets have been completed, then issue the next ticket.
```

---

## Simulator Rules

These rules apply to every ticket.

**You must:**
- Run read-only inspection commands before making any changes
- Explain what you are checking and why before Claude gives you any hints
- Apply the narrowest correct fix — no `chmod 777`, no over-permissioned shortcuts
- Verify the fix worked before closing the ticket
- Write a short resolution note before asking for the review

**Claude will:**
- Not reveal the answer until you demonstrate inspection
- Simulate realistic command output for risky scenarios
- Grade your approach honestly after each ticket
- Update the environment state file after each resolution
- Track your weak areas and target them in future tickets

---

## Difficulty Tiers

| Tier | Focus Area | Key Commands |
|---|---|---|
| 1 | Users, Groups, Permissions | `id`, `getent`, `useradd`, `usermod`, `chmod`, `chown`, `visudo` |
| 2 | Processes, Services, Logs | `ps`, `systemctl`, `journalctl`, `kill`, `pgrep` |
| 3 | Packages, Repos, Updates | `dnf`/`apt`, repo config, cache, verification |
| 4 | Networking and DNS | `ip`, `ss`, `ping`, `dig`, `curl`, firewall reasoning |
| 5 | Storage, Mounts, Disk Usage | `df`, `du`, `lsblk`, `fstab`, mount troubleshooting |
| 6 | SSH and Remote Access | `sshd_config`, key permissions, access policies |
| 7 | Automation | cron, systemd timers, Bash scripts, exit codes |
| 8 | Security Hardening | least privilege, service exposure, audit, SELinux basics |
| 9 | Mixed Incidents | multi-layer tickets combining tiers above |

Start at Tier 1 regardless of experience level. The point is depth and judgment, not speed.

---

## Ticket Workflow

Every ticket follows this cycle:

```
1. Ticket issued        — symptom described, no answer given
2. Inspection           — you run read-only commands, explain what you see
3. Diagnosis            — you identify root cause
4. Fix                  — you apply the correct change
5. Verification         — you confirm the fix worked
6. Resolution note      — short professional close
7. Review               — Claude grades your approach
8. Environment update   — state file reflects the change
9. Next ticket seeded   — new concept or targeted weak area
```

---

## Ticket Review Format

After each ticket, Claude produces a structured review:

```
Ticket:               Brief title
What I did well:      Strengths in your approach
Mistakes/Hesitations: Honest gaps
Commands Practiced:   List of commands used
Concepts Reinforced:  Linux+ relevant concepts
Correct Resolution:   The clean fix explained
Verification:         What to check after the fix
Resolution Note:      Professional close
Environment Update:   What changed in the state file
Weak Area Update:     New or updated weak areas
Next Recommended Ticket: Suggested follow-on
```

---

## Safety Guidelines by Tier

| Tier | Risk Level | Approach |
|---|---|---|
| 1–3 | Low | Run real commands on your server |
| 4 | Low-Medium | Real commands, careful with firewall changes |
| 5 | Medium | Real disk inspection, simulate fstab/mount changes |
| 6 | Medium | Real SSH inspection, simulate config changes on production |
| 7 | Low-Medium | Real cron/script work, test in safe directories |
| 8 | Medium-High | Simulate SELinux policy and service disabling on production |
| 9 | Medium-High | Assess per ticket |

**Always:**
- Have a snapshot or backup before Tier 5+ work
- Test firewall and SSH changes carefully — lock-out risk is real
- Use a disposable VM for anything involving partitioning, bootloader, or fstab

---

## Distro Reference

The simulator defaults to RHEL-family commands. If you are on Ubuntu/Debian, tell Claude at the start and it will adjust.

| Topic | RHEL Family | Debian Family |
|---|---|---|
| Package manager | `dnf` / `rpm` | `apt` / `dpkg` |
| Firewall | `firewalld` | `ufw` |
| SELinux | enforcing by default | AppArmor |
| Sudo group | `wheel` | `sudo` |
| Logs | `journalctl` | `journalctl` |
| Service manager | `systemctl` | `systemctl` |

---

## Linux+ Exam Alignment

This simulator maps directly to CompTIA Linux+ XK0-006 objectives:

- System management
- Security
- Scripting and automation
- Troubleshooting
- Linux operations and deployment

The ticket format specifically reinforces the troubleshooting methodology tested in Linux+ performance-based questions — which require you to work through a problem, not just select an answer.

---

## Cloud and SRE Relevance

The skills practiced here transfer directly to cloud operations work:

- Service failure diagnosis → cloud VM troubleshooting
- IAM and least privilege → GCP/AWS/Azure IAM
- Log investigation → cloud logging and observability
- Automation safety → production script standards
- Security hardening → cloud security baselines

The methodology — inspect first, form a hypothesis, verify before and after — is the same approach used in production incident response regardless of platform.

---

## FAQ

**Do I need a physical server?**
No. Any Linux VM works — VirtualBox, VMware, UTM, a cloud VM, a Raspberry Pi. As long as you can SSH in and run commands, it will work.

**What if I break something?**
Tier 1–3 tickets are low-risk and fully reversible. For higher tiers, Claude will simulate output for anything risky. Take a snapshot before Tier 5+ work regardless.

**Can I use this without the /linux-ticket skill?**
Yes — use the fallback prompt in the "Continuing Across Sessions" section. The skill just saves you from re-pasting that prompt every time.

**What if I'm on Windows running WSL?**
WSL works well. Treat your WSL environment as your Linux server. Note that WSL2 uses a different init system — Claude will adjust if you let it know.

**Does this work with Claude.ai (web)?**
The `/linux-ticket` skill requires Claude Code (the CLI). The simulator itself can be run manually via the web interface using the fallback prompts, but the skill-based workflow requires the CLI.

---

## License

MIT License — use it, fork it, share it.

---

*Built as a Linux+ exam prep and skill development project. Simulator design, environment state management, and ticket review format developed with Claude Code.*


--Happy Learning & Growing  -- Trace Dugar
