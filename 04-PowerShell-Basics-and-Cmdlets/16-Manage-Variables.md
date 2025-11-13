# PowerShell Variables — Study Notes

---

## Why Variables Matter

PowerShell’s pipeline lets you pass data through commands (get → filter → modify → output).  
But pipelines only move in one direction. You can’t store values for later steps.

Variables solve this problem.

A variable is a “box in memory” where you temporarily store:
- numbers
- strings
- dates
- objects (like AD users, services, files, etc.)

---

## What Variables Can Store

Variables can hold simple types OR full objects.  
Example: storing an AD user object lets you inspect all user properties later.

To see all variables currently in memory:

`Get-ChildItem Variable:`  
`Get-Variable`

---

# Naming Variables

- Use meaningful names: `$user`, `$logFilePath`, `$currentDate`
- Variables **always** start with `$`
- Avoid special characters
- Spaces require `{ }` → `${log File}` (not recommended)
- Not case sensitive: `$USER` = `$user`
- Recommended style: `$logFile`, `$CurrentUser`

---

# Assigning Values

Basic assignment:

`$num = 10`  
`$path = "C:\Logs\output.txt"`

Assign values from commands:

`$service = Get-Service W32Time`

If the command returns multiple items, the variable becomes an **array**.

Display a variable:

`$user`  
`Write-Host "The path is $path"`

Double quotes evaluate variables; single quotes do not.

Clear variables:

`$var = $null`  
or  
`Clear-Variable var`

---

# Using Variables in Math & Strings

`$area = $length * $width`  
`$sum = $a + $b`  
`$fullPath = $folder + $file`

Using Set-Variable (rarely needed):

`Set-Variable -Name num1 -Value 5`

---

# Variable Types

PowerShell usually auto-detects type:
- `5` → Int
- `"5"` → String
- `"January 5"` → DateTime

Force a type:

`[Int]$num = "5"`  
`[DateTime]$date = "January 5, 2022 10:00AM"`

Common types:

- String → text  
- Int → whole number  
- Double → decimal  
- DateTime → date/time object  
- Bool → `$true` / `$false`

Check a variable’s type:

`$date.GetType()`

---

# Object Properties & Methods

Variables can hold objects.  
Objects have:
- **properties** (data)  
- **methods** (actions)

View everything available:

`$object | Get-Member`

Or use tab-complete:

`$object.<TAB>`

Example:

`$user.DisplayName`  
`$user.Enabled`

---

# Working With Strings

Useful property:

`$string.Length`

Useful methods:
- `Contains("Admin")`
- `Insert(0,"Hello ")`
- `Remove(3,5)`
- `Replace("a","b")`
- `Split(" ")`
- `ToLower()`
- `ToUpper()`

Example:

`"Administrator".Contains("Admin")` → True  
`"Admin".ToUpper()` → ADMIN

---

# Working With DateTime

Properties:

`$date.Hour`  
`$date.DayOfWeek`  
`$date.Month`  
`$date.Year`  
`$date.Date`

Methods (add/subtract time):

`$date.AddDays(3)`  
`$date.AddDays(-30)`  
`$date.AddMonths(1)`  
`$date.AddHours(2)`

Formatting:

`$date.ToLongDateString()`  
`$date.ToShortDateString()`  
`$date.ToLongTimeString()`  
`$date.ToShortTimeString()`

---

# Real-World Examples

### Create timestamped log file
`$today = Get-Date`  
`$logName = "C:\Logs\server-" + $today.ToString("yyyyMMdd") + ".log"`

### Find inactive AD accounts (30 days)
`$cutoff = (Get-Date).AddDays(-30)`  
`Get-ADUser -Filter { LastLogonDate -lt $cutoff }`

### Build an email address
`$userName = "jsmith"`  
`$email = "$userName@contoso.com"`

### Store a full AD object and inspect it
`$user = Get-ADUser jsmith -Properties *`  
`$user.LastLogonDate`  
`$user.Enabled`  
`$user.DistinguishedName`

---

# Summary (What You Should Know)

- Variables store reusable data
- They can hold any object
- PowerShell usually auto-detects the type
- Use `Get-Member` to explore a variable’s capabilities
- Strings & DateTime objects have useful methods
- Date/time math is important for automation

