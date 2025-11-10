# PowerShell Modules and Help

# Step 1: Define Modules in PowerShell
Modules are groups of related PowerShell capabilities bundled together into a single unit.
They help organize cmdlets into distributable components that can be loaded or imported when needed.

Use this command to view all available modules:
```powershell
Get-Module -ListAvailable
```

To manually load a module:

```powershell
Import-Module <ModuleName>
```

# Step 2: Autoloading in PowerShell
Starting with Windows PowerShell 3.0 and PowerShell 7, modules load automatically when a cmdlet is used.
Module paths are stored in the environment variable:
```powershell
$env:PSModulePath
```

# Windows PowerShell default paths

%systemdir%\WindowsPowerShell\v1.0\Modules
%userprofiles%\Documents\WindowsPowerShell\Modules

# PowerShell 7 paths
C:\Users\<user>\Documents\PowerShell\Modules
C:\Program Files\PowerShell\Modules
C:\Program Files\PowerShell\7\Modules
C:\Program Files\WindowsPowerShell\Modules
C:\WINDOWS\System32\WindowsPowerShell\v1.0\Modules

## What is a Cmdlet?

A cmdlet (pronounced “command-let”) is a small, single-function command built into PowerShell.
It’s not a standalone program — instead, it runs inside the PowerShell engine.

Think of a cmdlet as a mini tool designed to perform one focused task, like:

Listing files, Creating users, Modifying system settings, Managing services, and so on.

# Step 3: Find Cmdlets in PowerShell
PowerShell includes thousands of cmdlets. To locate specific ones, use these discovery commands.

```powershell
# List all commands
Get-Command

# Filter by name pattern
Get-Command *event*

# Filter by verb or noun
Get-Command -Verb Get -Noun event*

# Find commands in a specific module
Get-Command -Module NetAdapter

# Step 4: Get Help for Cmdlets
PowerShell includes detailed built-in help for each cmdlet.

# View help for a specific command
Get-Help Get-ChildItem

# Show examples only
Get-Help Stop-Process -Examples

# View full detailed help
Get-Help Get-EventLog -Full

# Open help in a separate window or online
Get-Help Get-EventLog -ShowWindow
Get-Help Get-EventLog -Online

# Step 5: Using Show-Command
Show-Command opens a graphical window showing parameters for any cmdlet.
Example:
Show-Command Get-ADUser

This helps visualize available parameter sets and allows copying or running the command directly.

# Step 6: Using Aliases in PowerShell
Aliases are shortcuts or alternative names for cmdlets.

# View all aliases
Get-Alias

# Find a specific alias
Get-Alias dir

# Find what aliases map to a cmdlet
Get-Alias -Definition Remove-Item

# Create a new alias (temporary)
New-Alias del Remove-Item

# Note:
Aliases are not saved between sessions unless added to your PowerShell profile.
```
# Step 7: Understanding Help File Syntax
Each cmdlet’s help includes syntax definitions showing how parameters can be used.
```powershell
Example:
Get-EventLog [-LogName] <String> [[-InstanceId] <Int64[]>] [-After <DateTime>]
```

Square brackets [ ] indicate optional parameters.
Angle brackets < > indicate parameter types.
Parameters with [] inside their type (like <string[]>) accept multiple values.

# Step 8: Update Local Help
Windows PowerShell and PowerShell 7 use online help files.
```powershell
# Update help content for all modules
Update-Help

# Save help to an offline location
Save-Help -DestinationPath C:\Help

# Install saved help on offline systems
Update-Help -SourcePath C:\Help

Use -Force to bypass the 24-hour update restriction.
```

# Step 9: Common Assessment Answers
1. Use Get-Help Get-AD* to find cmdlets related to Active Directory.
2. Modules are groups of related PowerShell capabilities bundled into a single unit.

