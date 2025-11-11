# PowerShell Command Reference — Discovering and Exploring PowerShell

This reference organizes your `.ps1` training files into four main learning categories:
1. Searching for Cmdlets  
2. Using About Files (Help Topics)  
3. Working with Aliases  
4. Viewing and Managing Modules  

Each section includes:
- Purpose  
- Typical Usage Scenario  
- Example Commands (with expected behavior)

---

## 1. Search Cmdlets
**Purpose:** Find commands or cmdlets related to a specific keyword.

### Common Scenarios
- When you can’t remember the exact cmdlet name.
- To explore all available PowerShell commands matching part of a word.
- To narrow results to a specific cmdlet category.

### Commands

```powershell
# 1. Search for commands containing "net" in their name
Get-Command *net*

# 2. Search help files for cmdlets that include "net"
Get-Help *net* -Category cmdlet
```

**Explanation:**
- `Get-Command` searches across all cmdlets, functions, workflows, aliases, and scripts available in your PowerShell session.
- `Get-Help` limits search results to the PowerShell help system and filters by category (in this case, cmdlets).

**Expected Output:**
A list of cmdlets related to networking or containing “net,” such as `Get-NetAdapter`, `Get-NetIPAddress`, etc.

---

## 2. Using About Files
**Purpose:** Learn PowerShell concepts, behaviors, and syntax through built-in documentation.

### Common Scenarios
- Reviewing conceptual topics (not command-specific help).
- Discovering help about aliases, event logs, or scripting concepts.

### Commands

```powershell
# 1. List all available "about" help topics
Get-Help about*

# 2. Display help for aliases
Get-Help about_aliases

# 3. Open detailed help for event logs in a separate window
Get-Help about_eventlogs -ShowWindow

# 4. Search for help topics containing "beep"
Get-Help *beep*
```

**Explanation:**
- “About” topics are text-based documentation built into PowerShell that explain **concepts** like variables, loops, execution policy, etc.
- `-ShowWindow` opens the content in a resizable help viewer window.

**Expected Output:**
Text-based help files detailing the requested concept, displayed either in the console or in a separate window.

---

## 3. Using Aliases
**Purpose:** Understand and manage PowerShell aliases (shortcuts to cmdlets).

### Common Scenarios
- Discovering which commands have aliases (e.g., `dir`, `ls`, `cp`).
- Creating or modifying aliases for frequent use.
- Checking definitions and verifying which cmdlet an alias maps to.

### Commands

```powershell
# 1. Use an alias for directory listing
dir

# 2. Equivalent explicit cmdlet
Get-ChildItem

# 3. Find what command the alias 'dir' maps to
Get-Alias dir

# 4. Create a custom alias named 'list' for Get-ChildItem
New-Alias list Get-ChildItem

# 5. Test your new alias
list

# 6. Find all aliases that reference Get-ChildItem
Get-Alias -Definition Get-ChildItem
```

**Explanation:**
- `Get-Alias` shows existing aliases.
- `New-Alias` creates new ones (temporary until PowerShell is restarted).
- Aliases improve efficiency for interactive use but should be avoided in production scripts for clarity.

**Expected Output:**
Lists alias definitions or creates a new alias that works within the current session.

---

## 4. Viewing and Managing Modules
**Purpose:** Load, view, and explore PowerShell modules and their contents.

### Common Scenarios
- Checking what modules are currently loaded.
- Importing additional modules (like Active Directory or Server Manager).
- Listing available modules on the system.

### Commands

```powershell
# 1. Show modules currently loaded in your session
Get-Module

# 2. Run a command that loads a module automatically (e.g., Active Directory)
Get-ADUser Lara

# 3. Verify which modules are now loaded
Get-Module

# 4. List all modules installed and available for loading
Get-Module -ListAvailable

# 5. Manually import a module into your session
Import-Module ServerManager

# 6. Confirm the module has loaded
Get-Module
```

**Explanation:**
- `Get-Module` (no parameters) shows active modules.
- `-ListAvailable` reveals all modules on disk.
- `Import-Module` loads a module manually if it hasn’t autoloaded.

**Expected Output:**
Lists of module names, paths, and versions — confirming module availability or successful import.

---

## Summary Table

| Category | Key Cmdlets | Typical Use |
|-----------|--------------|-------------|
| Search Cmdlets | Get-Command, Get-Help | Find cmdlets or help topics |
| Using About Files | Get-Help about_* | Learn PowerShell concepts |
| Using Aliases | Get-Alias, New-Alias | Manage command shortcuts |
| Viewing Modules | Get-Module, Import-Module | Load and inspect PowerShell modules |

---

## Practical Order for Exploration
If you’re learning or troubleshooting PowerShell:

1. **Search** → Use `Get-Command` or `Get-Help` to find what you need.  
2. **Learn** → Open relevant “about” files for conceptual understanding.  
3. **Simplify** → Create or inspect aliases for convenience.  
4. **Expand** → Load and explore new modules for advanced cmdlets.

---

# PowerShell Command Reference — Networking and Active Directory

## 1. Network Configuration and Connectivity

**Purpose:** Verify connectivity, inspect configuration, and configure IP, DNS, and routing.

**Common Scenarios**
- Testing connectivity to servers or domain controllers  
- Assigning or updating static IP addresses  
- Configuring DNS servers  
- Managing default routes and validating changes  

### Commands

```powershell
# 1. Test connectivity to a specific host (lab example)
Test-Connection LON-DC1

# Target-agnostic: test any host
Test-Connection <ComputerNameOrIP>
# Example:
# Test-Connection google.com


# 2. View current IP configuration
Get-NetIPConfiguration


# 3. Add a new IP address to an interface (lab example)
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.30 -PrefixLength 16

# Remove an IP address (lab example)
Remove-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.40


# Target-agnostic: discover adapter, then assign/remove IP
# Step 1: List adapters to find the correct InterfaceAlias
Get-NetAdapter

# Step 2: Assign an IP address
New-NetIPAddress -InterfaceAlias "<AdapterName>" -IPAddress <IPAddress> -PrefixLength <PrefixBits>

# Step 3: Remove an IP address
Remove-NetIPAddress -InterfaceAlias "<AdapterName>" -IPAddress <IPAddress>
# Required info: adapter name, IP address, prefix length.


# 4. Set DNS server address (lab example)
Set-DnsClientServerAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.11

# Target-agnostic: set one or more DNS servers
Set-DnsClientServerAddress -InterfaceAlias "<AdapterName>" -ServerAddresses <DNS_IP_1>,<DNS_IP_2>
# Required info: adapter name, DNS server IP(s).


# 5. Remove default route (lab example)
Remove-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -Confirm:$false

# Target-agnostic: remove default route
Remove-NetRoute -InterfaceAlias "<AdapterName>" -DestinationPrefix 0.0.0.0/0 -Confirm:$false
# Required info: adapter name; verify it's the correct default route.


# 6. Add a default route with next hop (lab example)
New-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -NextHop 172.16.0.2

# Target-agnostic: add default route
New-NetRoute -InterfaceAlias "<AdapterName>" -DestinationPrefix 0.0.0.0/0 -NextHop <GatewayIP>
# Required info: adapter name, correct gateway IP.


# 7. Recheck current network configuration
Get-NetIPConfiguration


# 8. Retest connectivity
Test-Connection LON-DC1

# Target-agnostic: confirm access to key systems
Test-Connection <ComputerNameOrIP>

## 3. Active Directory User and Group Management
```
**Purpose:** Create groups and users, manage membership, and view or update user attributes.

**Common Scenarios**
- Creating new security or distribution groups  
- Adding users to existing groups  
- Viewing group memberships  
- Updating user address and contact information  
- Retrieving user attributes for documentation or audits  

### Commands

```powershell
# 1. Create a new Active Directory group (lab example)
New-ADGroup -Name HelpDesk -Path "ou=IT,dc=Adatum,dc=com" -GroupScope Global

# Target-agnostic: create any AD group
New-ADGroup -Name <GroupName> -Path "<OU_DN>" -GroupScope <Global|DomainLocal|Universal>
# Required info: OU Distinguished Name, desired group scope.


# 2. Create a new AD user (lab example)
New-ADUser -Name "Jane Doe" -Department "IT"

# Target-agnostic
New-ADUser -Name "<UserDisplayName>" -Department "<Department>"
# Optional parameters: -SamAccountName, -UserPrincipalName, -AccountPassword, -Enabled $true.


# 3. Add users to a group (lab example)
Add-ADGroupMember "HelpDesk" -Members "Lara","Jane Doe"

# Target-agnostic
Add-ADGroupMember "<GroupName>" -Members <User1>,<User2>,<User3>
# Required info: exact group name and user identifiers.


# 4. View members of a group (lab example)
Get-ADGroupMember HelpDesk

# Target-agnostic
Get-ADGroupMember <GroupName>


# 5. Update a user’s address information (lab example)
Set-ADUser Lara -StreetAddress "1530 Nowhere Ave." -City "Winnipeg" -State "Manitoba" -Country "CA"

# Target-agnostic
Set-ADUser <UserName> -StreetAddress "<Address>" -City "<City>" -State "<StateOrProvince>" -Country "<CountryCode>"
# Required info: user name and correct address details.


# 6. View all groups a user belongs to (lab example)
Get-ADPrincipalGroupMembership "Jane Doe"

# Target-agnostic
Get-ADPrincipalGroupMembership <UserName>
# Displays all groups the specified user is a member of.


# 7. Retrieve specific user attributes (lab example)
Get-ADUser Lara -Properties StreetAddress,City,State,Country

# Target-agnostic
Get-ADUser <UserName> -Properties StreetAddress,City,State,Country
# Useful for verification after updating user details.
```