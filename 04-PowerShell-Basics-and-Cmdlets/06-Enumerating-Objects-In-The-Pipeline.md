# 🔁 Enumerate Objects in the Pipeline

---

## 🧩 Step 1. Understanding Enumeration

**Enumeration** is the process of performing an action on each object, one at a time, in a collection.

In many cases, PowerShell automatically handles enumeration for you.  
For example:

```powershell
Get-Process -Name Notepad | Stop-Process
```

is functionally identical to:

```powershell
Stop-Process -Name Notepad
```

---

### ⚙️ When Explicit Enumeration Is Needed
Explicit enumeration becomes necessary when:

- You need to call a **method** that has no corresponding cmdlet.  
- You want to perform **custom actions** on each object in a pipeline.

#### Example
Encrypt all files in a folder using the `.Encrypt()` method (part of the `System.IO.FileInfo` object):
```powershell
Get-ChildItem -File | ForEach-Object -MemberName Encrypt
```

---

## ⚙️ Step 2. Basic Enumeration Syntax

Use the **`ForEach-Object`** cmdlet (aliases: `ForEach`, `%`) to enumerate objects.

### Basic Syntax
```powershell
Get-ChildItem -Path C:\Encrypted\ -File | ForEach-Object -MemberName Encrypt
```

### Shorter Versions
```powershell
Get-ChildItem -Path C:\Encrypted\ -File | ForEach Encrypt
Get-ChildItem -Path C:\Encrypted\ -File | % Encrypt
```

💡 **Note:**  
Parentheses are **not** used after the method name in basic syntax.

---

### ⚠️ Limitations
The basic syntax can:
- Access **only one property or method**.  
- **Cannot** perform logic (`-and`, `-or`, or comparisons).  
- **Cannot** run multiple commands or script blocks.

Example of invalid syntax:
```powershell
Get-Service | ForEach -MemberName Stop -and -MemberName Close  # ❌ Error
```

---

## 🧠 Step 3. Advanced Enumeration Syntax

The advanced form of **`ForEach-Object`** provides full control by running a **script block**.  
This block executes **once for each object** in the pipeline.

### Example
```powershell
Get-ChildItem -Path C:\ToEncrypt\ -File | ForEach-Object -Process { $PSItem.Encrypt() }
```

Each time through the pipeline:
- `$PSItem` (or `$_`) refers to the current object.  
- The code inside `{ }` runs for each object.

---

### 🧩 Key Notes
- When calling methods in advanced syntax, **always include parentheses**, even if no parameters exist:
  ```powershell
  $PSItem.Encrypt()
  ```
- If the method accepts arguments, place them **inside the parentheses**, separated by commas.

---

## ⚡ Step 4. Advanced Enumeration Techniques

### 1️⃣ Using the Range Operator (`..`)

The **range operator (`..`)** generates a sequence of integers.  
You can use it with `ForEach-Object` to repeat actions a set number of times.

#### Example
```powershell
1..100 | ForEach-Object { Get-Random }
```

Explanation:
- The range `1..100` generates numbers from **1 to 100**.  
- Each number is passed through the pipeline.  
- The script block runs **100 times**, executing `Get-Random` each time.  
- The integers themselves aren’t used — they just control repetition.

---

## 📘 Quick Reference

| Concept | Description | Example |
|----------|--------------|----------|
| **Enumeration** | Perform an action on each object in a collection | `ForEach-Object` |
| **Cmdlet** | The command that performs enumeration | `Get-Service` |
| **Aliases** | Shortcuts for `ForEach-Object` | `ForEach`, `%` |
| **Range Operator** | Creates a sequence of numbers | `1..10` |
| **Pipeline Variable** | Represents the current object | `$PSItem` or `$_` |

---

## 🧩 Knowledge Check

**Q1:** Which symbol represents the range operator?  
**A:** `..`

**Q2:** Which symbol represents the alias of the `ForEach-Object` cmdlet?  
**A:** `%`
