[← Journal home](../README.md) · [Portfolio](https://traceportfolio.netlify.app/)

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/entries/linux-ticket-resolutions-dusk.svg">
  <source media="(prefers-color-scheme: light)" srcset="../assets/entries/linux-ticket-resolutions-day.svg">
  <img alt="Linux ticket practice: two recorded resolutions — sailboat illustration" src="../assets/entries/linux-ticket-resolutions-day.svg" width="1200">
</picture>

# Linux ticket practice: two recorded resolutions

<sub>May 2, 2026 · RHEL troubleshooting and least privilege</sub>

## Overview

These two exercises put the [Linux Admin Ticket Queue](../April%2026/LinuxAdminTicketQueue.md) into practice on PaleHorse, my [headless RHEL lab](../March%2026/RHELServer.md). Claude Code issued controlled Tier 1 scenarios involving fictional accounts and deliberately seeded faults. AI mentoring supported my investigation, command use, and resolution notes.

The objective was to practice a complete troubleshooting process: inspect the relevant state, explain the diagnosis, make a targeted change, verify the result, and record what I needed to learn next.

## TICK-001: narrowing an account's privileges

The initial inspection showed a fictional junior-administrator account in the `wheel` group. The lab's recorded sudoers policy gave that group unrestricted sudo access, making its membership relevant to the exercise.

With AI mentoring and the `usermod` documentation, I worked through a targeted membership change. My resolution note records removing the practice account from `wheel`. The later group listing shows only the lab administrator remaining.

```text
<lab_admin>@<lab_host>:~$ id jrodriguez
uid=1001(jrodriguez) gid=1001(jrodriguez) groups=1001(jrodriguez),10(wheel)

<lab_admin>@<lab_host>:~$ grep wheel /etc/group
wheel:x:10:<lab_admin>
```

<sub>Retained, user-pasted inspection: the account's initial groups and the later wheel membership. The real administrator and host identifiers are redacted.</sub>

This comparison supports the group-membership correction under the recorded lab policy. It does not establish that every possible source of sudo access was audited. The exercise exposed a learning gap: connecting group membership to the configured sudo policy and checking the correct group file.

## TICK-002: restoring interactive login

The next exercise concerned a fictional developer account that could not complete an interactive login. The completion log identifies a `nologin` shell and a missing home directory. My retained notes describe inspecting the account, investigating the missing home, and an initial `mkdir`/`chown` attempt.

With mentoring, I learned how ownership and default home files affect an account. My closure note records setting Bash, correcting the home with `mkhomedir_helper`, applying owner-only permissions (`700`), and successfully testing SSH.

```text
<lab_admin>@<lab_host>:/etc/ssh$ getent passwd mthompson
mthompson:x:1002:1002::/home/mthompson:/bin/bash
```

<sub>Retained, user-pasted account output after the shell change. The real administrator and host identifiers are redacted.</sub>

The surviving output shows the Bash account entry. The successful SSH test is documented in my closure note; a raw SSH success transcript was not retained. Keeping those forms of evidence distinct makes the record more useful and accurate.

## Lessons and follow-up

These exercises reinforced several habits:

- Connect a symptom to the relevant account, ownership, group, or policy before changing it.
- Use documentation and AI mentoring to understand a command's effect.
- Prefer the narrowest correction that fits the intended access.
- Record both the result and the gaps that should guide the next exercise.

Two completed Tier 1 exercises demonstrate this part of the workflow. The broader nine-tier queue describes the intended practice scope, rather than a claim that every tier has been completed. Future exercises should carry these learning gaps forward and retain stronger before-and-after verification where useful.

[Read the public case study and retained evidence](https://traceportfolio.netlify.app/case-studies/linux-ticket-queue).

---

[← Journal home](../README.md) · [Portfolio](https://traceportfolio.netlify.app/)
