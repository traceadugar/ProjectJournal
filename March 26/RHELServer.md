# RHEL Server

## Overview
- This project repurposes an office desktop (HP ProDesk G2) into a headless RHEL server for homelab use.
- The goal is to simulate a real-world enterprise server environment on physical hardware.
- Focus areas include Linux administration, remote access, and system-level experimentation.
- This build supports hands-on learning aligned with cloud and security engineering paths.

---

## Components & Requirements
### Hardware
- HP ProDesk 600 G2
- Intel i7-6700 (upgraded)
- 1TB SSD (upgraded from 128GB)
- 16GB RAM
- 32GB Thumb Drive

### Software
- Red Hat Enterprise Linux (RHEL)
- Rufus

### Network / Environment
- Integrated into existing homelab network
- Headless operation (no monitor/peripherals after setup)
- Remote management via SSH

---

## Setup Process
1. Replaced stock CPU and storage with upgraded components and created a bootable RHEL installation drive using Rufus.
2. System initially halted during POST due to fan detection error; manually bypassed to proceed with installation. Replacement fan ordered for resolution.
3. Verified system BIOS version and confirmed it was already up to date.
4. Completed RHEL installation and performed initial console-based configuration.
5. Activated network interface using `nmcli` and verified connectivity.
6. Confirmed external network access with successful ICMP ping to Google.
7. Registered system and activated RHEL subscription using developer account.
8. Configured SSH for remote access and transitioned to headless operation.
9. Enabled network auto-connect to ensure persistence across reboots.
10. Installed core utility packages for administration and monitoring.

---

## Key Configurations
- Network configuration performed using `nmcli` to bring up interface `eno1` and assign a static IP address.
- Static IP assignment ensures consistent remote access and reliable lab environment behavior.
- SSH configured to enable remote management from primary workstation, eliminating need for direct physical access.
- Network auto-connect enabled to prevent interface drop after reboot.
- Core packages installed:
  - `vim` – text editing
  - `git` – version control
  - `curl`, `wget` – data transfer and testing
  - `net-tools` – legacy networking utilities
  - `bind-utils` – DNS troubleshooting tools (`dig`, `nslookup`)
  - `bash-completion` – CLI usability enhancement
  - `tmux` – session persistence
  - `btop` – system resource monitoring (compiled from source due to repository limitations)

---

## Validation & Testing
- Verified network connectivity using:
  - `ping google.com`
- Confirmed DNS resolution using `dig`
- Established successful SSH session from external machine
- Verified system resource visibility and stability using `btop`
- Confirmed network persistence and accessibility after reboot

---

## Troubleshooting
- Fan detection error during POST:
  - System required manual confirmation to continue boot
  - Identified as hardware-level compatibility issue after CPU upgrade
  - Resolution in progress via replacement fan
- Network interface not active after initial setup:
  - Resolved using `nmcli` to bring interface up and enable auto-connect
- Package availability limitations on RHEL:
  - `btop` not available in default repositories
  - Resolved by compiling from source

---

## Security Considerations
- Remote access restricted to SSH-based management
- System designed to operate without direct physical interaction
- Static IP assignment allows for controlled network access and segmentation within homelab
- Default enterprise Linux security controls (firewall and SELinux) retained

---

## Optimization
- Headless configuration reduces system overhead and resource consumption
- SSD upgrade significantly improves system responsiveness and IO performance *********
- Static IP eliminates dependency on DHCP and reduces connection instability
- Lightweight toolset installed to maintain minimal and efficient environment

---

## Lessons Learned
- Enterprise Linux distributions require additional repositories or manual builds for certain tools
- Hardware upgrades can introduce compatibility issues at the firmware level
- Network persistence must be explicitly configured in headless environments
- Building from source is sometimes necessary in restricted or minimal environments
- Establishing a clean baseline system simplifies future troubleshooting and expansion

---

## Reusable Patterns
- Converting legacy office hardware into functional lab infrastructure
- Establishing a headless Linux server with persistent remote access
- Using `nmcli` for reliable network configuration in server environments
- Handling repository limitations through source compilation
- Structuring systems for repeatable deployment and documentation
