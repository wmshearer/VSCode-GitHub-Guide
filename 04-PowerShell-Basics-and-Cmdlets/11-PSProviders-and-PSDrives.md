# PowerShell Command Reference — Providers

## What Is a PowerShell Provider?

**Simple definition:**  
A **PowerShell provider** is a component that lets you browse and manage different types of data (files, registry, certificates, environment variables, etc.) **using the same commands** you use for the file system.

Instead of learning a different tool for each data store, providers let you:
- `Set-Location` (`cd`) into them  
- Run `Get-ChildItem` (`dir`, `ls`)  
- Use `Get-Item`, `Set-Item`, `Remove-Item`, etc.

**Key idea:**  
Providers turn many kinds of data into **PowerShell-accessible “locations.”**

## Common Built-In Providers (Examples)

- **FileSystem** → Drives like `C:\`, `D:\`  
- **Registry** → `HKLM:\`, `HKCU:\`  
- **Env** → `Env:\` (environment variables)  
- **Cert** → `Cert:\` (certificate stores)  
- **Alias** → `Alias:\` (PowerShell aliases)  
- **Function** → `Function:\` (PowerShell functions)  
- **Variable** → `Variable:\` (PowerShell variables)

## Commands

```powershell
# 1. List all available providers
Get-PSProvider

# 2. View details for a specific provider (example: Registry)
Get-PSProvider Registry

# 3. See which providers support special capabilities
Get-PSProvider | Select-Object Name, Capabilities

# 4. Show providers and their drives
Get-PSProvider |
Select-Object Name, @{n='Drives';e={ ($_.Drives | Select-Object -ExpandProperty Name) -join ', ' }}

# 5. Explore a provider like a file system (examples)
Set-Location HKLM:\
Get-ChildItem

Set-Location Env:\
Get-ChildItem

Set-Location Cert:\LocalMachine\My
Get-ChildItem
```

## Notes


Providers define what kind of data you can access.


PSDrives (next section) define where you enter that data (like HKLM:\, Env:\, Cert:\).

```powershell
You still use the same core cmdlets:

Get-ChildItem (dir, ls)

Get-Item, Set-Item, Remove-Item

Set-Location (cd)

Use Get-PSProvider anytime you’re unsure what’s available on a system.
```
# PowerShell Command Reference — Drives (PSDrives)

## What Is a PowerShell Drive?

**Simple definition:**  
A **PowerShell drive (PSDrive)** is a *shortcut or access point* that connects to a data location exposed by a **provider**.  
You can navigate these drives just like folders in File Explorer — even though some of them don’t store files.

**Key idea:**  
PSDrives let you treat everything (registry, certificates, environment variables, etc.) like a file system.

---

## Common Built-In PSDrives

| PSDrive | Provider | Description |
|----------|-----------|--------------|
| `C:\` | FileSystem | Standard file drive. |
| `HKLM:\` | Registry | HKEY_LOCAL_MACHINE registry hive. |
| `HKCU:\` | Registry | HKEY_CURRENT_USER registry hive. |
| `Env:\` | Environment | System and user environment variables. |
| `Cert:\` | Certificate | Local and user certificate stores. |
| `Alias:\` | Alias | All defined PowerShell command aliases. |
| `Function:\` | Function | PowerShell function definitions. |
| `Variable:\` | Variable | Currently defined PowerShell variables. |

---

## Commands

```powershell
# 1. List all current PSDrives
Get-PSDrive

# 2. View only FileSystem drives
Get-PSDrive -PSProvider FileSystem

# 3. Explore the Registry drive
Set-Location HKLM:\Software
Get-ChildItem

# 4. Explore Environment Variables
Set-Location Env:\
Get-ChildItem

# 5. Access Certificates
Set-Location Cert:\LocalMachine\My
Get-ChildItem

# 6. Create a custom PSDrive (temporary mapping)
New-PSDrive -Name Tools -PSProvider FileSystem -Root "C:\AdminTools"

# 7. Verify your new drive
Get-PSDrive Tools

# 8. Remove a custom PSDrive
Remove-PSDrive -Name Tools
```
## Notes

PSDrives exist because of providers — without a provider, there’s no drive.

PSDrives can be temporary (removed when PowerShell closes) or persistent (saved in profiles).

New-PSDrive is often used to:

- Map a local or network folder to a shorter path.
- Connect to a registry location, certificate store, or remote file share.
- All the same commands (cd, dir, ls, Get-ChildItem) work inside any PSDrive.

Example: Creating and Navigating PSDrives
```powershell
# Create a new drive called "Logs" for quick access
New-PSDrive -Name Logs -PSProvider FileSystem -Root "C:\Windows\System32\LogFiles"

# Move into the new drive
Set-Location Logs:

# List contents
Get-ChildItem
```