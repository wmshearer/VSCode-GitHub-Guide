# ⚙️ Select, Sort, and Measure Objects Using the Pipeline

---

## 🧩 Step 1. Sort and Group Objects by Property

Use the **`Sort-Object`** cmdlet (alias: `sort`) to reorder command output.  
By default, PowerShell performs a **case-insensitive sort in ascending order**.  
To reverse the order, use the **`-Descending`** parameter.

You can sort by multiple properties — PowerShell sorts by the first, then the next, and so on.

### 🔹 Examples
```powershell
Get-Service | Sort-Object -Property Name -Descending
Get-Service | Sort Name -Desc
Get-Service | Sort Status, Name
```

To sort ignoring or respecting case, use parameters like **`-CaseSensitive`**.  
Sorting rules can also follow specific cultures (locale-aware sorting).

---

### 🧮 Grouping Objects

Use the **`-GroupBy`** parameter of formatting cmdlets (`Format-List`, `Format-Table`, or `Format-Wide`) to group data by a property.  
Alternatively, use **`Group-Object`** (alias: `group`) for more control.

#### Example
```powershell
Get-Service | Sort-Object Status, Name | Format-Wide -GroupBy Status
```

---

## 📊 Step 2. Measure Objects in the Pipeline

The **`Measure-Object`** cmdlet (alias: `measure`) counts and calculates numeric data from pipeline objects.  
By default, it counts the number of objects in a collection.

Use the **`-Property`** parameter to perform calculations on numeric properties.

| Parameter | Description |
|------------|-------------|
| `-Sum` | Adds all values |
| `-Average` | Computes the mean |
| `-Minimum` | Finds the smallest value |
| `-Maximum` | Finds the largest value |

### Example
```powershell
Get-ChildItem -File | Measure -Property Length -Sum -Average -Minimum -Maximum
```

> 💡 **Tip:** You can shorten parameters like `-Minimum` → `-Min` and `-Maximum` → `-Max`,  
> but not `-Average` (you can use `-Ave` instead).

---

## 🔍 Step 3. Select a Set of Objects in the Pipeline

The **`Select-Object`** cmdlet (alias: `select`) can limit or customize command output.  
Think of it as choosing **rows (objects)** from the pipeline.

| Parameter | Description |
|------------|-------------|
| `-First` | Returns the first *n* objects |
| `-Last` | Returns the last *n* objects |
| `-Skip` | Skips a specific number of objects |
| `-Unique` | Returns only unique objects (removes duplicates) |

### 🔹 Examples
```powershell
# First 10 processes using least virtual memory
Get-Process | Sort-Object -Property VM | Select-Object -First 10

# Last 10 services sorted by name
Get-Service | Sort-Object -Property Name | Select-Object -Last 10

# Skip the lowest CPU process, get next 5
Get-Process | Sort-Object -Property CPU -Descending | Select-Object -First 5 -Skip 1
```

### 🧩 Unique Selection Example
```powershell
Get-ADUser -Filter * -Property Department |
Sort-Object -Property Department |
Select-Object Department -Unique
```

---

## 🧱 Step 4. Select Object Properties in the Pipeline

Use **`Select-Object -Property`** to choose which **columns (properties)** to display.  
If a property isn’t shown by default, verify its actual name using `Get-Member`.

### Example
```powershell
Get-Process | Select-Object -Property Name, ID, VM, PM, CPU | Format-Table
```

### Combining with Sorting and Limits
```powershell
Get-Process | Sort-Object -Property CPU -Descending | Select-Object -Property Name, CPU -First 10
```

> 🧩 **Note:** Some column headers displayed in default tables aren’t the actual property names  
> (e.g., **CPU(s)** is really **CPU**). Always verify property names using:
```powershell
Get-Process | Get-Member
```

---

## 🧮 Step 5. Create and Format Calculated Properties

`Select-Object` can create **calculated (custom) properties** using **hash tables**.

Each hash table defines:
- **Label (or Name, n, l)** → the property’s display name  
- **Expression (or e)** → a script block defining the value

### Example
```powershell
Get-Process |
Select-Object Name, ID,
              @{n='VirtualMemory'; e={$PSItem.VM}},
              @{n='PagedMemory';  e={$PSItem.PM}}
```

---

### 💾 Formatting Values

To convert and format memory sizes in megabytes:
```powershell
Get-Process |
Select-Object Name, ID,
              @{n='VirtualMemory(MB)'; e={$PSItem.VM / 1MB}},
              @{n='PagedMemory(MB)';  e={$PSItem.PM / 1MB}}
```

To display with two decimal places:
```powershell
Get-Process |
Select-Object Name, ID,
              @{n='VirtualMemory(MB)'; e={'{0:N2}' -f ($PSItem.VM / 1MB)}},
              @{n='PagedMemory(MB)';  e={'{0:N2}' -f ($PSItem.PM / 1MB)}}
```

> 🧠 `$PSItem` represents the current object in the pipeline.  
> Older syntax uses `$_`, which still works for compatibility.

---

## ⚡ Quick Reference

| Cmdlet | Purpose | Alias |
|---------|----------|--------|
| `Sort-Object` | Sorts objects by property | `sort` |
| `Group-Object` | Groups objects by property | `group` |
| `Measure-Object` | Counts and calculates numeric values | `measure` |
| `Select-Object` | Selects specific objects or properties | `select` |

---

## 🧩 Knowledge Check

**Q1:** What is the default behavior of `Sort-Object`?  
**A:** Case-insensitive sort in ascending order.

**Q2:** Which cmdlet is used to choose displayed properties?  
**A:** `Select-Object`
