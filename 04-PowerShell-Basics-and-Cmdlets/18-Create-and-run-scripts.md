# Create and Run Scripts in Windows PowerShell — Study Notes

---

# Why Scripts Matter

When you first use PowerShell, you run single commands. But as tasks get more complex, repeating commands becomes:
- Slow  
- Error-prone  
- Inconsistent  

Scripts solve these problems because:
- They **automate** repetitive tasks  
- They **standardize** procedures  
- They allow **complex logic**, like `if`, `foreach`, `switch`, reporting, loops, and more  
- They can be **scheduled** to run automatically  

Examples:
- Reporting disk space on all servers  
- Scanning Exchange message logs  
- Updating hundreds of AD accounts  
- Restarting groups of services  

PowerShell scripts use the **.ps1** file extension and are simply text files containing PowerShell commands.

---

# Editing Scripts

You can edit scripts with:
- **Visual Studio Code** (best choice today)
- **PowerShell ISE** (older)
- **Notepad** (basic)

Even though you *can* use Notepad, VS Code provides:
- Syntax highlighting  
- Debugging  
- IntelliSense  
- Git integration  

---

# Modifying Existing Scripts

Most admins start by **modifying someone else’s script**, not writing their own.

Sources of scripts:
- PowerShell Gallery  
- Microsoft documentation  
- Online forums, blogs, GitHub  
- Code samples from the community  

Always:
- Review the script  
- Understand what it does  
- Test in a **non-production** environment  

Even well-intentioned scripts can contain bugs or behave unexpectedly.

---

# Creating Your Own Scripts

If no script fits your needs, you can build one.

Guidelines:
1. **Build scripts in steps**, not all at once  
2. **Test each section separately**  
3. **Use a lab environment**, not production  
4. **Start small**, like modifying one test user  
5. Once each piece works, assemble the full script  

Example approach:
- Step 1: Get all users from a group  
- Step 2: Make the change to **one** user  
- Step 3: Apply the change to all users only after testing  

---

# Understanding the PowerShellGet Module

PowerShellGet contains cmdlets that let you search for and install:
- Modules  
- Scripts  
- DSC resources  

Most common cmdlets:

| Cmdlet | Purpose |
|--------|---------|
| **Find-Module** | Search PowerShell Gallery for modules |
| **Find-Script** | Search PowerShell Gallery for scripts |

When using these for the first time, PowerShell prompts you to install **NuGet**, which PowerShellGet depends on.

---

# TLS 1.2 Requirement

PowerShell Gallery requires **TLS 1.2** for security.

Enable TLS 1.2 temporarily:

`[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`

Enable it permanently:

```powershell
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Wow6432Node\Microsoft.NetFramework\v4.0.30319' -Name 'SchUseStrongCrypto' -Value '1' -Type DWord
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft.NetFramework\v4.0.30319' -Name 'SchUseStrongCrypto' -Value '1' -Type DWord
```


---

# Private PowerShell Gallery

Organizations can create their own private repository using:
- A file share  
- A web-based NuGet server  

To register a private repository:
`Register-PSRepository -Name "MyRepo" -SourceLocation "\\Server\Share"`

---

# Running PowerShell Scripts

You **cannot** run a PowerShell script by double-clicking it.  
This is a security feature.

Double-clicking a `.ps1` file opens it in **Notepad**, preventing accidental execution.

Context menu options when right-clicking a script:
- **Open** → open in Notepad  
- **Edit** → open in PowerShell ISE  
- **Run with PowerShell** → runs the script but closes the window afterward  

Best practice:
**Run scripts from a PowerShell window that is already open**, so you can see the output.

---

# How to Run a Script

PowerShell does **not** search the current directory automatically.

These methods work:

- Full path:  
  `C:\Scripts\MyScript.ps1`

- Relative path:  
  `..\Scripts\MyScript.ps1`

- Current directory:  
  `.\MyScript.ps1`

---

# Execution Policy

Execution Policy determines whether you can run scripts on a system.

Check current policy:

`Get-ExecutionPolicy`

### Execution Policy Options

| Policy | Description |
|--------|-------------|
| **Restricted** | No scripts allowed |
| **AllSigned** | Only digitally signed scripts allowed |
| **RemoteSigned** | Downloaded scripts must be signed; local scripts can run unsigned |
| **Unrestricted** | All scripts allowed, warnings for downloaded scripts |
| **Bypass** | No restrictions or warnings |

Set the execution policy:

`Set-ExecutionPolicy RemoteSigned`

### Override policy only for this session:

`powershell.exe -ExecutionPolicy Bypass`

### Remove "downloaded" status from a file:

`Unblock-File .\myscript.ps1`

---

# AppLocker and Script Security

Execution Policy offers basic protection but is easy to bypass.

**AppLocker** provides stronger control:
- Restrict scripts by path  
- Restrict scripts by publisher  
- Allow only scripts signed by your organization  

In PowerShell 5.0 and later:
- When AppLocker Allow rules apply, PowerShell enters **ConstrainedLanguage** mode  
- This allows basic scripting but blocks dangerous actions (like calling arbitrary .NET methods)  

This helps prevent attackers from abusing PowerShell.

---

# Digitally Signing Scripts

Digitally signing scripts ensures:
- Only approved, trusted scripts run  
- Scripts cannot be changed without breaking the signature  
- Helps enforce **AllSigned** execution policy  

To sign a script, you need a **code-signing certificate**.

Example:

```powershell
$cert = Get-ChildItem -Path "Cert:\CurrentUser\My" -CodeSigningCert
Set-AuthenticodeSignature -FilePath "C:\Scripts\MyScript.ps1" -Certificate $cert
```


If the script changes, you must sign it again.

---

# Module Assessment

## 1. Which execution policy prevents unsigned **downloaded** scripts from running?

Correct answer: **RemoteSigned**

Explanation:  
- RemoteSigned blocks *downloaded* unsigned scripts  
- Local scripts can run unsigned

---

## 2. Which cmdlet adds a digital signature to a script?

Correct answer: **Set-AuthenticodeSignature**

Explanation:  
This cmdlet attaches a certificate-based signature to a script file.

