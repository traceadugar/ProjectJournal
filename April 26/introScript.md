# PaleHorse Intro Script

## Overview
This script generates a custom terminal intro screen for my RHEL machine, **PaleHorse**, on login. It combines ASCII art, system metadata, and structured color output to create a clean, intentional startup experience.

This was a **self-directed side quest project**, built while learning Bash. I wrote it alongside AI-assisted iteration, using prompting to refine output, alignment, color behavior, and formatting until I got the result I wanted.

---

## What This Does
- Clears the terminal for a clean entry point
- Renders a stylized ASCII horse with layered grayscale accents
- Displays a custom **PaleHorse** logo in red
- Surfaces key system data immediately:
  - Uptime
  - CPU Load
  - Linux Distribution
  - OS Version

---

## Why I Built This
Most systems I’ve worked on display either very minimal login information or cluttered, low-UX startup output.

I wanted something that:
- Feels intentional and owned
- Prioritizes readability and structure
- Gives immediate system awareness
- Reflects the identity of the machine
- Reinforces my Bash and terminal formatting skills

---

## Key Concepts Used

### Environment Awareness
```bash
. /etc/os-release
```

- Dynamically loads distro name and version
- Reflects real-world system introspection practices

### Command Parsing

```bash
UPTIME=$(uptime -p)
LOAD=$(uptime | awk -F'load average: ' '{print $2}')
```

- Extracts human-readable uptime
- Uses `awk` to isolate load averages cleanly

### 256-Color Terminal Control

```bash
LIGHT_GRAY=$'\033[38;5;245m'
DARK_GRAY=$'\033[38;5;238m'
RED=$'\033[38;5;124m'
INFO_GRAY=$'\033[38;5;247m'
LABEL_RED=$'\033[38;5;160m'
NC=$'\033[0m'
```

- Uses extended ANSI (256-color) for finer control
- Separates visual layers intentionally:
  - Art (grayscale)
  - Labels (red)
  - Data (soft gray)

### Inline Text Transformation

```bash
line="${line//%/${LIGHT_GRAY}%${NC}}"
line="${line//O/${DARK_GRAY}O${NC}}"
line="${line//C/${DARK_GRAY}C${NC}}"
```

- Dynamically injects color into ASCII art
- Treats characters as semantic elements:
  - `%` → mane/tail
  - `O` → eye
  - `C` → nostril

### Structured Output Formatting

```bash
printf "%bUptime:%b %b%-30s%b %bDistro:%b %b%s%b\n" \
  "${LABEL_RED}" "${NC}" "${INFO_GRAY}" "$UPTIME" "${NC}" \
  "${LABEL_RED}" "${NC}" "${INFO_GRAY}" "$NAME" "${NC}"
```

- Aligns output into readable columns
- Creates a clean separation between labels and values
- Keeps the boot screen informative without becoming cluttered

---

## AI-Assisted Development

This project was built using **intentional prompting alongside hands-on scripting**.

I used AI to help:

- Iterate on color schemes
- Debug string replacement and formatting issues
- Refine layout and spacing
- Improve readability and output structure
- Move faster through small implementation changes

The key point is that I was not just accepting outputs blindly. I was **guiding the interaction toward a specific result**, adjusting prompts and making decisions based on what I wanted the final script to do and look like.

This demonstrates growing skill in:

- Prompting for technical outcomes
- Rapid iteration
- Human-in-the-loop development
- Using AI as a development multiplier

---

## Sources

- Horse ASCII: https://www.asciiart.eu/animals/horses
- Horse credited as: **unknown**
- Font generator for `PaleHorse`: https://patorjk.com/software/taag/
- ASCII font used: **Classy**

---

## What This Enables

- Immediate system awareness on login
- Cleaner workflow before starting work
- Reusable patterns for future scripts:
  - formatting
  - parsing
  - color handling
  - terminal presentation

---

## Reflection

This is a small script, but it demonstrates:

- Attention to detail
- Strong terminal UX thinking
- Ability to turn learning into something functional quickly
- Ownership over both function and presentation

It also shows that I can:

- Build self-directed projects outside the course path
- Use AI effectively to refine technical work
- Combine system administration concepts with visual design instincts
- Improve my environment in ways that are both practical and intentional

While the ASCII art itself is static, the underlying structure is modular and reusable across future tooling.
