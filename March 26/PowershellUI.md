# PowerShell Personalization

## Script
![Powershell UI](/Images/PowerShell%Customization.png)
---
script {

        $Host.UI.RawUI.BackgroundColor = 'Black'
        $Host.UI.RawUI.ForegroundColor = 'Gray'
        Clear-Host
        
        Set-PSReadLineOption -Colors @{
            Command   = 'Cyan'
            Parameter = 'DarkYellow'
            String    = 'Green'
            Operator  = 'Magenta'
            Variable  = 'White'
            Number    = 'DarkCyan'
            Member    = 'DarkCyan'
            Type      = 'Gray'
            Comment   = 'DarkGreen'
            Keyword   = 'DarkMagenta'
            Error     = 'Red'
            Selection = 'DarkGray'
        }
        
        Set-PSReadLineOption -BellStyle None
        Set-PSReadLineKeyHandler -Key Tab -Function MenuComplete
        
        function prompt {
        
            $isAdmin = ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole(
                [Security.Principal.WindowsBuiltInRole]::Administrator
            )
        
            if ($isAdmin) {
                Write-Host "[ADMIN] " -NoNewline -ForegroundColor DarkGreen
            }
        
            $time = Get-Date -Format "HH:mm"
        
            Write-Host "MachineName " -NoNewline -ForegroundColor Cyan
            Write-Host "$(Split-Path -Leaf (Get-Location)) " -NoNewline -ForegroundColor DarkYellow
            Write-Host "$time" -ForegroundColor Gray
            
            Write-Host ">> " -NoNewline -ForegroundColor Magenta
            return " " 
        }
}
---

## Overview
- This project customizes the PowerShell terminal to improve readability, reduce friction, and create a more intentional command-line workspace.
- The goal was not only visual personalization, but also functional improvement through better syntax highlighting, cleaner prompt information, and faster command completion behavior.
- This setup reflects the idea that a terminal should feel like a purpose-built tool rather than a default utility.
- The customization supports day-to-day command-line use by making important information easier to see at a glance.

---

## Architecture
- The profile script runs when PowerShell starts.
- It first sets the terminal background and foreground colors, then clears the screen so the new visual settings apply cleanly.
- It configures PSReadLine syntax highlighting so different parts of commands are easier to distinguish.
- It disables the terminal bell and changes the Tab key behavior to menu-based completion.
- It replaces the default prompt with a custom prompt that shows:
  - Whether the session is running as administrator
  - The machine identity through the `MachineName` label
  - The current folder name
  - The current time
  - A custom prompt marker

---

## Components & Requirements

### Hardware
- Windows PC

### Software
- PowerShell

### Environment
- PowerShell profile script
- Interactive terminal use on Windows

---

## Setup Process
1. Open the PowerShell profile file.
2. Add terminal color settings for background and foreground.
3. Clear the terminal after applying those visual defaults.
4. Configure PSReadLine syntax colors for commands, parameters, strings, operators, variables, numbers, members, types, comments, keywords, errors, and selection.
5. Disable the bell so the shell behaves more quietly during use.
6. Change the Tab key to use menu completion for easier command and path discovery.
7. Define a custom `prompt` function.
8. In the prompt function:
   - Check whether the session is elevated
   - Display an admin indicator when applicable
   - Capture the current time
   - Display the host label, current directory name, and time
   - Print a custom prompt symbol
9. Save the profile and restart PowerShell to test the result.

---

## Key Configurations

### Base Terminal Colors
- `$Host.UI.RawUI.BackgroundColor = 'Black'`
  - Sets the terminal background to black.
  - This creates a high-contrast base that is easy to read.
- `$Host.UI.RawUI.ForegroundColor = 'Gray'`
  - Sets the default text color to gray.
  - This softens the terminal output compared to bright white while keeping it readable.
- `Clear-Host`
  - Clears the screen after applying the colors.
  - This ensures the updated visual settings are immediately reflected in the terminal window.

### PSReadLine Syntax Highlighting
- `Set-PSReadLineOption -Colors @{ ... }`
  - Assigns colors to different token types in PowerShell input.
  - This makes commands easier to parse visually before execution.
  - Examples:
    - Commands are cyan
    - Parameters are dark yellow
    - Strings are green
    - Operators are magenta
    - Errors are red
    - Comments are dark green
- Why it matters:
  - It improves readability.
  - It reduces mistakes by making command structure easier to interpret quickly.
  - It gives the shell a more refined and intentional feel.

### Bell Behavior
- `Set-PSReadLineOption -BellStyle None`
  - Disables the terminal bell.
  - This removes unnecessary audible or visual interruptions during command-line work.

### Tab Completion Behavior
- `Set-PSReadLineKeyHandler -Key Tab -Function MenuComplete`
  - Changes the Tab key from standard completion to menu-based completion.
  - This is useful when exploring commands, parameters, or file paths because it shows available options more clearly.

### Custom Prompt Function
- `function prompt { ... }`
  - Overrides the default PowerShell prompt.
  - This allows the terminal to display more useful session context.

#### Admin Check
- The prompt checks whether the current PowerShell session is running with administrator privileges.
- If true, it prints:
  - `[ADMIN]`
- Why it matters:
  - It gives immediate visibility into session privilege level.
  - This helps prevent confusion when running commands that behave differently in elevated vs non-elevated shells.

#### Time Display
- `$time = Get-Date -Format "HH:mm"`
  - Captures the current time in 24-hour format.
- Why it matters:
  - It gives quick context while working in long terminal sessions.
  - It is useful when tracking work, commands, or troubleshooting flow.

#### Host and Location Display
- `Write-Host "MachineName " ...`
  - Displays the terminal identity label.
  - In this case, it reinforces the machine identity and makes the shell feel personalized.
- `Write-Host "$(Split-Path -Leaf (Get-Location)) " ...`
  - Displays only the current folder name rather than the full path.
  - This keeps the prompt clean and compact.

#### Prompt Symbol
- `Write-Host ">> " -NoNewline -ForegroundColor Magenta`
  - Displays a custom prompt marker.
  - This gives the shell a more deliberate visual style than the default prompt.

---

## Validation & Testing
- Confirm the terminal opens with a black background and gray default text.
- Type sample commands and verify syntax colors appear as expected.
- Test Tab completion and confirm a menu of options appears.
- Launch PowerShell as administrator and verify the `[ADMIN]` label appears.
- Change directories and confirm the prompt updates to the current folder name.
- Check that the current time appears in the prompt.
- Verify that the prompt ends with the custom `>>` marker.

---

## Troubleshooting

### Profile Changes Do Not Appear
- Confirm the script was saved in the correct PowerShell profile path.
- Restart PowerShell after saving the profile.
- Check for syntax errors in the profile script.

### Colors Do Not Display Correctly
- Confirm the terminal host supports the configured colors.
- Verify no other profile settings are overriding the PSReadLine color map.

### Tab Completion Does Not Show MenuComplete
- Confirm PSReadLine is available and loaded.
- Verify the key handler line is present and has no syntax issues.

### Admin Label Never Appears
- Make sure PowerShell is actually launched with elevated privileges.
- Test by opening PowerShell with "Run as administrator."

---

## Security Considerations
- The prompt includes an admin indicator, which helps reduce the chance of forgetting when the shell is elevated.
- This is useful because administrative sessions carry more risk when running destructive commands.
- The profile does not introduce remote execution, credential storage, or privileged automation by itself.
- As with any profile customization, changes should be reviewed carefully to avoid introducing unsafe commands at shell startup.

---

## Optimization
- Showing only the current folder name keeps the prompt compact and easier to scan.
- Including the time provides useful context without excessive clutter.
- Using menu completion improves usability, especially when exploring commands or working with long paths.
- The color palette balances readability with visual separation of syntax elements.

---

## Lessons Learned
- Terminal customization can improve both aesthetics and functionality at the same time.
- Small usability changes, such as better syntax highlighting and cleaner prompts, reduce friction during repeated daily use.
- Including context like privilege level and time makes the shell more informative without making it overly busy.
- A good terminal setup should support speed, clarity, and awareness rather than just looking different.
