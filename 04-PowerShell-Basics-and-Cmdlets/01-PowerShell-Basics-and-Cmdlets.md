# 05_PowerShell_Basics_and_Cmdlets

## PowerShell Versions
- **Windows PowerShell (5.1)** → `powershell.exe`
- **PowerShell 7+** → `pwsh.exe`
- PowerShell 7 installs separately and can run side-by-side with 5.1
- Profiles, module paths, and event logs are separate for each version

## Run as Administrator
- Right-click PowerShell → **Run as Administrator**
- The window title will include “Administrator” when elevated

## Execution Policy
View current policy:
Get-ExecutionPolicy

Change policy:
Set-ExecutionPolicy -ExecutionPolicy <PolicyName>

Common policies:
- Restricted → Default on Windows clients; no scripts allowed  
- RemoteSigned → Default on Windows servers; local scripts OK, downloaded scripts need a signature  
- AllSigned → Only runs signed scripts  
- Unrestricted → Runs all scripts (shows warning for external sources)  
- Undefined → No policy set; defaults to system type (client/server)

## Console Customization
- Change font, size, and colors in console **Properties**
- Adjust screen buffer width/height in the **Layout** tab
- Enable **QuickEdit Mode** and **Ctrl+C / Ctrl+V** shortcuts under **Options → Edit Options**

## Integrated Scripting Environment (ISE)
- Provides script editor, console, and command add-on pane
- Supports themes, snippets, and debugging
- **Note:** ISE is deprecated — use **Visual Studio Code** with the PowerShell extension instead

## Visual Studio Code with PowerShell
- Install VS Code + PowerShell extension
- Supports PowerShell 5.1, 6, and 7+
- Enable “ISE Mode” to mimic ISE shortcuts and appearance

## What is a Cmdlet?

A cmdlet (pronounced “command-let”) is a small, single-function command built into PowerShell.
It’s not a standalone program — instead, it runs inside the PowerShell engine.

Think of a cmdlet as a mini tool designed to perform one focused task, like:

Listing files, Creating users, Modifying system settings, Managing services, and so on.

## Cmdlet Structure
- Cmdlets follow **Verb-Noun** format  
  Examples:  
  Get-Process  
  Set-ExecutionPolicy  
  New-Item

Common verbs:
Get, Set, New, Add, Remove

List all approved verbs:
Get-Verb

## Cmdlet Parameters
- Start with a dash (-)
- Separate parameter and value with a space
- Wrap values in quotes if they contain spaces
- Some parameters are **positional**, so names can be omitted
- **Switch parameters** (like `-Recurse`) act as True/False toggles

Example:
Get-ChildItem -Path C:\ -Recurse

## Tab Completion
- Press **Tab** to auto-complete cmdlet names, parameters, or file paths
- Press **Tab** repeatedly to cycle through matches
- Works with wildcards, e.g.:
*-service  → cycles through cmdlets containing “service”

## About Files
List all conceptual help topics:
Get-Help about*

Read a specific topic:
Get-Help about_common_parameters

Open in window or browser:
Get-Help about_common_parameters -ShowWindow
Get-Help about_common_parameters -Online
