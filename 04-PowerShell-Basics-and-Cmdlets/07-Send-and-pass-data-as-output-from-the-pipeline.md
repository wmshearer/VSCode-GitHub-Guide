# 📤 Send and Pass Data as Output from the Pipeline

---

## 🧩 Step 1. Write Pipeline Data to a File

The **`Out-File`** cmdlet writes pipeline output to a file.  
It captures exactly what would display on-screen and saves it as text.

```powershell
Get-Service | Out-File -FilePath "ServiceList.txt"
```

### 📝 Notes
- `Out-File` renders objects as **text**, not as objects.  
- The result is intended for **human reading**, not for reuse in scripts.  
- After `Out-File` runs, **nothing remains** in the pipeline (no screen output).

### ⚙️ Common Parameters
| Parameter | Description |
|------------|--------------|
| `-FilePath` | Specifies the file name or path |
| `-Append` | Adds content instead of overwriting |
| `-Encoding` | Sets file encoding (`UTF8`, `ASCII`, etc.) |

---

### 🔁 Redirection Operators
PowerShell supports `cmd.exe`-style redirection:

| Operator | Description |
|-----------|-------------|
| `>` | Writes (overwrites) |
| `>>` | Appends |

```powershell
Get-Process > Processes.txt        # Overwrites file
Get-Process >> Processes.txt       # Appends to file
```

---

### 🧮 Example Pipeline
```powershell
Get-Service |
Sort-Object -Property Status, Name |
Select-Object -Property DisplayName, Status |
Out-File -FilePath ServiceList.csv
```

| Stage | Output Type |
|--------|--------------|
| `Get-Service` | System.ServiceProcess.ServiceController |
| `Sort-Object` | Same object type (sorted) |
| `Select-Object` | Selected.System.ServiceProcess.ServiceController |
| `Out-File` | No output — pipeline ends |

💡 **Debug Tip:**  
If a long pipeline fails, build it **step-by-step** and verify each command’s output before chaining the next.

---

## ⚙️ Step 2. Convert Pipeline Objects to Other Data Forms

PowerShell can **convert** or **export** objects into formats like **CSV**, **XML**, or **JSON**.

### Convert vs. Export
| Verb | Action | Example |
|------|---------|----------|
| `ConvertTo-*` | Converts objects and keeps data in memory | `ConvertTo-Csv` |
| `Export-*` | Converts and writes directly to file | `Export-Csv` |

---

### 📊 Convert and Export CSV
```powershell
Get-Service | ConvertTo-Csv | Out-File Services.csv
Get-Service | Export-Csv Services.csv
```

- `ConvertTo-Csv` → leaves data **in memory** (for further processing).  
- `Export-Csv` → **converts + writes** to disk.  
- CSV includes **headers** (property names).  
- `Import-Csv` → re-creates PowerShell objects from CSV files.

---

### 🧱 Convert and Export XML
```powershell
Get-Service | Export-Clixml Services.xml
```

XML is **structured and portable**, ideal for data exchange with other systems.

| Cmdlet | Description |
|---------|--------------|
| `ConvertTo-Clixml` | Converts to XML (keeps in memory) |
| `Export-Clixml` | Converts + saves to file |

XML preserves **hierarchy** and supports **multi-value properties** easily.

---

### 🧩 Convert to JSON
```powershell
Get-Service | ConvertTo-Json | Out-File Services.json
```

- JSON is lightweight and ideal for **web applications**.  
- PowerShell does **not** include an `Export-Json` cmdlet.  
- Use `ConvertTo-Json` + `Out-File` (or `>` operator) to save.

---

### 🌐 Convert to HTML
```powershell
Get-Service | ConvertTo-Html -Title "Service Report" | Out-File Services.html
```

Creates HTML **tables or lists** for use in reports or emails.

#### Useful Parameters
| Parameter | Description |
|------------|-------------|
| `-Head` | Content for the `<head>` section |
| `-Title` | Sets the `<title>` tag |
| `-PreContent` / `-PostContent` | Add text before or after the table |

---

## 🖥️ Step 3. Control Additional Output Options

### 🧾 Out-Host
Displays output one page at a time.
```powershell
Get-ADUser | Out-Host -Paging
```
Pauses output after each screenful of data.

---

### 🖨️ Out-Printer
Sends pipeline output directly to a printer.
```powershell
Get-Service | Out-Printer
```

Or specify a printer name:
```powershell
Get-Service | Out-Printer -Name "Microsoft Print to PDF"
```

---

### 🪟 Out-GridView
Displays data in an **interactive GUI window** with sort and filter options.
```powershell
Get-Process | Out-GridView
```

You can:
- Sort and filter columns.  
- Copy or search dynamically.  
- ❌ Cannot save directly from the grid, but you can copy or pipe data elsewhere.

---

## 📘 Quick Reference

| Cmdlet | Description | Output Type |
|---------|--------------|-------------|
| `Out-File` | Writes text output to file | None |
| `Export-Csv` | Converts objects and writes to CSV | None |
| `Export-Clixml` | Converts objects and writes to XML | None |
| `ConvertTo-Json` | Converts to JSON (in memory) | String |
| `ConvertTo-Html` | Converts to HTML table | String |
| `Out-Host -Paging` | Displays output one page at a time | Console |
| `Out-Printer` | Sends output to a printer | None |
| `Out-GridView` | Displays data in an interactive grid | GUI |

---

## 🧩 Knowledge Check

**Q1:** Which cmdlet should a user pipe output to for paging results?  
**A:** `Out-Host -Paging`

**Q2:** Which symbol overwrites file content using Out-File alias?  
**A:** `>`
