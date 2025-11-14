# PowerShell User Input — Study Notes

## Values in Scripts That Might Change

When you first write a script, it usually solves a specific problem at a specific time. Examples:

- Find AD users who haven’t signed in for 60 days.
- Search domain controllers for certain event IDs from the last 30 days.

Later, you might need variations:

- Use 30 days instead of 60.
- Search different servers instead of just domain controllers.
- Use different event IDs.

Hard-coding these values means you must keep editing the script. That’s bad when:

- Multiple admins share the same scripts.
- Scripts are digitally signed (you’d have to re-sign after every change).

Better approaches:

- Put “changeable” values in variables at the top of the script.
- Even better: **accept user input at runtime** instead of editing the script each time.

This is where `Read-Host`, `Get-Credential`, GUI selection (`Out-GridView`), and script parameters come in.

---

## Read-Host — Simple User Text Input

`Read-Host` pauses the script and asks the user for input, then stores the response in a variable.

Example: ask user “How many days?” and store the answer.

```powershell
$answer = Read-Host "How many days"
```

The user sees:

How many days:

They type a value and press Enter, and that value is stored in `$answer`.

### Avoiding the automatic colon

`Read-Host` always appends a colon (`:`) to the prompt text.  
If you don’t want that, combine `Write-Host` and `Read-Host`:

```powershell
Write-Host "How many days? " -NoNewline
$answer = Read-Host
```

### Input length limit

- `Read-Host` input is limited to **1022 characters**.

### Masking input (for sensitive values)

You can hide what the user types (show `*`):

- `-MaskInput` → stores as plain String, but input is masked.
- `-AsSecureString` → stores as SecureString (needed for passwords).

```powershell
# Masked but stored as plain text (String)
$token = Read-Host "Enter API token" -MaskInput

# Stored as SecureString (good for passwords)
$password = Read-Host "Enter password" -AsSecureString
```

Use `-AsSecureString` for anything credential-like that shouldn’t exist as clear text in memory.

---

## Get-Credential — Prompt for Username and Password

Best practice for admins:

- Use a **regular account** for daily work.
- Use a **separate admin account** for elevated tasks.

`Get-Credential` allows a script to prompt for creds and then use those creds for specific commands.

Basic pattern:

```powershell
$cred = Get-Credential
Set-ADUser -Identity $user -Department "Marketing" -Credential $cred
```

Behavior:

- Pops up a standard Windows credential UI.
- User enters username + password.
- Result is a **PSCredential object** stored in `$cred`.

You can customize the message and pre-fill the username:

```powershell
$cred = Get-Credential -Message "Enter admin account" -UserName "CONTOSO\AdminUser"
```

Now you can pass `$cred` into cmdlets that support the `-Credential` parameter.

---

## Storing Credentials with Export-Clixml (Local Use)

Sometimes you don’t want to be prompted every run. You can **save** a credential to disk securely with `Export-Clixml`.

```powershell
$cred = Get-Credential
$cred | Export-Clixml C:\Scripts\cred.xml
```

Later, load it back:

```powershell
$cred = Import-Clixml C:\Scripts\cred.xml
```

Security behavior:

- The credential is **encrypted**.
- It can **only** be decrypted by:
  - The same user
  - On the same computer

Good for: personal automation scripts.  
Not good for: sharing among multiple admins or machines.

---

## Storing Credentials with SecretManagement (Shared or Vault-Based)

For shared or more advanced secret handling, use the **SecretManagement** module.

Install from PowerShell Gallery:

```powershell
Install-Module Microsoft.PowerShell.SecretManagement
```

SecretManagement can work with multiple vaults, such as:

- KeePass
- LastPass
- CredMan
- Azure Key Vault

Microsoft also provides **SecretStore** to create a **local vault** (similar to Export-Clixml, bound to user/machine).

Use case:

- Store service account creds in a vault.
- Scripts retrieve secrets when needed instead of hardcoding passwords.

(Details of setting up vaults are out of scope here, but this is the modern, recommended direction.)

---

## Out-GridView — GUI Selection Menu

`Out-GridView` is normally used to visually view data, but with `-PassThru` or `-OutputMode`, it becomes a **selection UI**.

Example: let the user pick one or more users from a list.

```powershell
$selection = $users | Out-GridView -Title "Select users to modify" -PassThru
```

Behavior:

- Opens a grid window with `$users` displayed.
- User selects one or more rows and clicks OK.
- The selected objects are stored in `$selection`.

Then you can process only the selected items.

```powershell
ForEach ($user in $selection) {
    Write-Host "Modifying user $($user.Name)"
}
```

### Controlling selection with -OutputMode

`-OutputMode` gives more control than just `-PassThru`:

- `None` → nothing is returned to the pipeline (view-only).
- `Single` → user can select zero or one row.
- `Multiple` → user can select zero, one, or many rows (same as `-PassThru`).

Example:

```powershell
$selection = $users | Out-GridView -Title "Select ONE user" -OutputMode Single
```

Important:

- Users are not forced to select anything.
- Always handle the case where `$selection` is `$null` or empty.

---

## Script Parameters — The Professional Way to Accept Input

The most robust and “PowerShell-like” way to accept input is by **parameters**, just like cmdlets.

You define parameters with a `Param()` block at the top of your script:

```powershell
Param(
    [string]$ComputerName,
    [int]$EventID
)
```

When you do this, your script gets named parameters:

- `-ComputerName`
- `-EventID`

Example of calling the script:

```powershell
.\GetEvent.ps1 -ComputerName LON-DC1 -EventID 5772
```

Parameters are positional by default:

- If you run: `.\GetEvent.ps1 LON-DC1 5772`
- `LON-DC1` → first parameter (`$ComputerName`)
- `5772` → second parameter (`$EventID`)

---

## Passing Data Without Param() — Using $args (Not Recommended)

If you don’t define a `Param()` block:

- Values passed after the script name end up in `$args`.

Example call:

```powershell
.\MyScript.ps1 LON-DC1 5772
```

Inside the script:

- `$args[0]` = `"LON-DC1"`
- `$args[1]` = `"5772"`

This works, but named parameters with `Param()` are **far better** and easier to understand.

---

## Defining Types in Param() for Validation

Defining types in your `Param()` block lets PowerShell auto-validate input.

Example:

```powershell
Param(
    [string]$ComputerName,
    [int]$EventID
)
```

If a user tries to pass a non-integer into `-EventID`, PowerShell throws an error.  
This is a built-in sanity check.

---

## Switch Parameters — On/Off Flags

Use a `[switch]` parameter for true/false options that act like flags.

Example: a `-Quiet` parameter to hide output.

```powershell
Param(
    [switch]$Quiet
)

If (-not $Quiet) {
    Write-Host "Script is running..."
}
```

Usage:

- `.\script.ps1 -Quiet` → `$Quiet` is `$true`
- `.\script.ps1` → `$Quiet` is `$false`

User doesn’t have to type `$true` or `$false`.  
This is cleaner than `[bool]` for parameters.

---

## Default Values for Parameters

You can assign a default value in the `Param()` block:

```powershell
Param(
    [string]$ComputerName = "LON-DC1"
)
```

If the user doesn’t specify `-ComputerName`, it defaults to `"LON-DC1"`.

---

## Prompting in Param() When No Value Is Provided

You can also force a prompt if the user doesn’t provide a parameter:

```powershell
Param(
    [int]$EventID = Read-Host "Enter event ID"
)
```

If the script is run without `-EventID`, the user is asked for a value.

---

## Module Assessment

### 1. To accept named parameters, what must be added to a script?

Correct answer:  
A **Param() block with variable names defined**.

Reason:  
The variables inside `Param()` become the script’s parameter names.

---

### 2. Which cmdlet is used to collect a username and password?

Correct answer:  
`Get-Credential`

Reason:  
`Get-Credential` shows a secure credentials dialog and returns a PSCredential object.

