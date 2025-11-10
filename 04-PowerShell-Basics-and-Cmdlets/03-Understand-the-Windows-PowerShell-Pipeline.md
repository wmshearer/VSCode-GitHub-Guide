# 🧠 Understand the Windows PowerShell Pipeline

---

## 🧩 Step 1. Review the PowerShell Pipeline and Its Output

A **pipeline** is a chain of one or more commands where the **output of one command becomes the input for the next**.  
Commands in a pipeline are separated by the pipe symbol (`|`) and executed **left to right**.

Each command line you enter is a **single pipeline** — PowerShell processes it, displays the output, and waits for the next command.

### Example
```powershell
Get-Service | Where-Object {$_.Status -eq 'Running'} | Select-Object Name, DisplayName
```

> 💡 **Tip:** You’ll often use `Get-*` commands as input for `Set-*` commands.  
> `Where` and `Select` are aliases for `Where-Object` and `Select-Object`.

---

### 🧠 Objects in PowerShell

PowerShell doesn’t output text — it outputs **objects**, which are structured pieces of data with **properties** (attributes) and **methods** (actions).

Think of a command’s output like a spreadsheet:

| Spreadsheet Term | PowerShell Equivalent |
|------------------|------------------------|
| Row              | Individual Object      |
| Column           | Object Property        |

#### Example
```powershell
Get-Service
```

This outputs a **collection of service objects** with properties like `Name`, `DisplayName`, and `Status`.

**Why this matters:**  
Unlike text-based shells, PowerShell eliminates the need for text parsing — it passes **structured objects** between commands.

---

## 🧱 Step 2. Discover Object Members in PowerShell

Objects contain **members**, which include:

- **Properties** – Describe object attributes (e.g., `Name`, `Status`, `ID`)
- **Methods** – Perform actions (e.g., `Stop()`, `Start()`, `Clear()`)
- **Events** – Trigger when something happens (e.g., file updates, process completion)

### View all members of an object
```powershell
Get-Service | Get-Member
```

Alias: `gm`

Displays all **properties**, **methods**, and the **type name** of the object  
(for example: `System.ServiceProcess.ServiceController`).

> ⚠️ **Tip:** Avoid combining `-WhatIf` with `Get-Member`.  
> `-WhatIf` suppresses output, meaning `Get-Member` receives nothing.

---

## 🎨 Step 3. Control the Formatting of Pipeline Output

PowerShell automatically formats output, but you can control it using **formatting cmdlets**:

| Cmdlet | Description | Alias |
|---------|--------------|--------|
| `Format-List` | Displays each property on a new line (best for detailed output). | `fl` |
| `Format-Table` | Displays data in a table (best for comparing multiple objects). | `ft` |
| `Format-Wide` | Displays a single property in multiple columns (compact). | `fw` |
| `Format-Custom` | Uses XML for custom layouts (advanced use). | — |

---

### 🧾 Format-List Example
```powershell
Get-Process | Format-List
```
- Displays each property on its own line.  
- Ideal for reviewing many properties at once.

---

### 🧮 Format-Table Example
```powershell
Get-ADObject -Filter * -Properties * |
Format-Table -Property Name, ObjectClass, Description -AutoSize -Wrap
```
- Displays data in a structured table.  
- **Common parameters:**
  - `-AutoSize` adjusts column width.
  - `-HideTableHeaders` removes headers.
  - `-Wrap` wraps long text.

---

### 📋 Format-Wide Example
```powershell
Get-GPO -All | Format-Wide -Property DisplayName -Column 3
```
Displays a single property (like names) across multiple columns —  
perfect for large, simple lists.

---

## ⚡ Quick Reference

| Concept | Description |
|----------|--------------|
| **Pipeline** | Transfers command output as input to the next command. |
| **Object** | Structured data with properties and methods. |
| **Get-Member** | Displays all members (properties/methods) of an object. |
| **Format-List** | Displays properties line-by-line. |
| **Format-Table** | Displays properties in columns. |
| **Format-Wide** | Displays one property in multiple columns. |

---

## 🧩 Knowledge Check

**Q1:** What is a pipeline in PowerShell?  
**A:** A chain of one or more commands where the output from one command passes as input to the next.

**Q2:** Which formatting cmdlet displays each property on a new line?  
**A:** `Format-List`
