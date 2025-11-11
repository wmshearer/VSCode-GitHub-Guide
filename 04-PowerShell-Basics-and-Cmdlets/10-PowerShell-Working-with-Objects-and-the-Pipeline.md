# PowerShell Command Reference — Working with Objects and the Pipeline

## Basic Enumeration

**Purpose:** Introduce object iteration using the PowerShell pipeline and `ForEach` / `ForEach-Object` to process multiple items efficiently.

**Common Scenarios**
- Looping through services, files, or logs to read or modify values.  
- Automating bulk actions (e.g., clearing logs or checking properties).  
- Accessing object properties directly from pipeline results.  

### Commands

```powershell
# 1. List service names (lab example)
Get-Service | ForEach Name

# Target-agnostic
# Enumerate all objects from Get-Service and return only the Name property.
Get-Service | ForEach-Object { $_.Name }
# Alternative form using alias:
Get-Service | % { $_.Name }


# 2. Clear the System event log (lab example)
Get-EventLog -List | Where Log -eq 'System' | ForEach Clear

# Target-agnostic
# Find specific event logs and clear them dynamically.
Get-EventLog -List | Where-Object { $_.Log -eq '<LogName>' } | ForEach-Object { $_.Clear() }
# Example:
# Get-EventLog -List | Where-Object { $_.Log -eq 'Application' } | ForEach-Object { $_.Clear() }
# Required info: log name (e.g., System, Application, Security).
```

## Advanced Enumeration

**Purpose:** Demonstrate how to iterate through objects in the pipeline using `ForEach-Object` and perform operations on each item.

**Common Scenarios**
- Modify or transform properties of multiple objects dynamically.  
- Perform actions (such as creating folders or updating registry keys) on each object returned by a command.  
- Combine enumeration with filters like `Where-Object` for targeted results.  

### Commands

```powershell
# 1. Modify network drive properties (lab example)
Get-ItemProperty -Path HKCU:\Network\* |
ForEach-Object -Process {
    Set-ItemProperty -Path $PSItem.PSPath -Name RemotePath -Value $PSItem.RemotePath.ToUpper()
}

# Target-agnostic
# Enumerate registry items, process each, and modify a property value.
Get-ItemProperty -Path <RegistryPathPattern> |
ForEach-Object -Process {
    Set-ItemProperty -Path $PSItem.PSPath -Name <PropertyName> -Value <NewValueExpression>
}
# Required info: registry path pattern, property name, new value logic.


# 2. Create subdirectories for matching folders (lab example)
Get-ChildItem E:\ -Directory -Recurse |
Where Name -eq Democode |
ForEach-Object { $PSItem.CreateSubdirectory('Test') } |
Select-Object FullName

# Target-agnostic
# Recursively find directories by name and create a new subfolder in each.
Get-ChildItem <DriveOrPath> -Directory -Recurse |
Where Name -eq <FolderName> |
ForEach-Object { $PSItem.CreateSubdirectory('<SubfolderName>') } |
Select-Object FullName
# Required info: drive/path, folder name, new subfolder name.

```

## Converting and Exporting Data

**Purpose:** Demonstrate how to convert PowerShell objects into various file formats (HTML, JSON, CSV, XML) for reporting, sharing, or external use.

**Common Scenarios**
- Exporting process or service information into readable or shareable formats.  
- Generating HTML or CSV reports for documentation or troubleshooting.  
- Saving structured data for re-importing or automation purposes.  

### Commands

```powershell
# 1. Convert process data to HTML (in-memory)
Get-Process | ConvertTo-HTML


# 2. Convert process data to HTML and export to a file
Get-Process | ConvertTo-HTML | Out-File E:\Procs.html

# Target-agnostic
Get-Process | ConvertTo-HTML | Out-File <FilePath>
# Example:
# Get-Process | ConvertTo-HTML | Out-File "C:\Reports\Processes.html"
# Required info: destination file path.


# 3. Convert process data to JSON and export
Get-Process | ConvertTo-JSON > C:\Procs.json

# Target-agnostic
Get-Process | ConvertTo-Json | Out-File <FilePath>
# Example:
# Get-Process | ConvertTo-Json | Out-File "C:\Reports\Processes.json"


# 4. Export service data to CSV
Get-Service | ConvertTo-CSV | Out-File Serv.csv

# or more efficiently:
Get-Service | Export-CSV E:\Serv.csv

# Target-agnostic
Get-Service | Export-CSV <FilePath> -NoTypeInformation
# Required info: destination file path.
# -NoTypeInformation removes metadata from CSV headers.


# 5. View the CSV file
Notepad E:\Serv.csv
# Target-agnostic
Notepad <FilePath>


# 6. Export service data to XML
Get-Service | Export-CliXML E:\Serv.xml

# Target-agnostic
Get-Service | Export-CliXML <FilePath>
# Useful for preserving full object structure (not just text output).


# 7. View the XML file
Notepad E:\Serv.xml
# Target-agnostic
Notepad <FilePath>
```
## Notes
```powershell
ConvertTo-* cmdlets transform objects within memory — use Out-File, redirection (>), or Export-* to save to disk.

Export-* cmdlets are optimized for saving directly to files and support richer data retention (especially Export-Clixml).

Common export formats and their uses:

HTML → For human-readable reports.

JSON → For web or automation integrations.

CSV → For Excel and text-based analysis.

XML → For full object preservation and re-importing.
```

## Creating Calculated Properties

**Purpose:** Demonstrate how to extend object output with new, on-the-fly calculated values using `Select-Object` and custom expressions.

**Common Scenarios**
- Adding new columns to reports (e.g., account age, disk usage percentage).  
- Performing real-time math or logic without modifying original data.  
- Combining property selection, sorting, and filtering for detailed analysis.  

### Commands

```powershell
# 1. List all Active Directory users and include the 'whenCreated' property
Get-ADUser -Filter * -Properties whenCreated

# Target-agnostic
Get-ADUser -Filter * -Properties <PropertyName>
# Required info: specify which AD user properties to include.


# 2. Sort AD users by creation date (newest first)
Get-ADUser -Filter * -Properties whenCreated |
Sort-Object -Property whenCreated -Descending

# Target-agnostic
Get-ADUser -Filter * -Properties <PropertyName> |
Sort-Object -Property <PropertyName> -Descending
# Example: sort users by whenCreated, LastLogonDate, or other attributes.


# 3. Add a calculated property showing account age in days
Get-ADUser -Filter * -Properties whenCreated |
Sort-Object -Property whenCreated -Descending |
Select-Object -Property Name, @{n='Account age (days)';e={(New-TimeSpan -Start $PSItem.whenCreated).Days}}

# Target-agnostic
Get-ADUser -Filter * -Properties <DateProperty> |
Sort-Object -Property <DateProperty> -Descending |
Select-Object -Property <DisplayProperty>, @{n='<CustomColumnName>';e={(New-TimeSpan -Start $PSItem.<DateProperty>).Days}}
# Example:
# Add a calculated column showing how many days an account has existed.
```
## Notes
```powershell
@{n='';e={}} syntax defines a hashtable for calculated properties:

n → Name (the column title)

e → Expression (the logic used to compute the value)

The New-TimeSpan cmdlet calculates time differences (e.g., account age).

You can use multiple calculated properties in one Select-Object call for richer reports.
```

## Filtering

**Purpose:** Use `Where-Object` and filtering operators to limit, refine, or evaluate objects that pass through the pipeline.

**Common Scenarios**
- Displaying only certain SMB shares or disks that match criteria.  
- Filtering for objects in a specific health or operational state.  
- Matching cmdlet verbs or names using wildcard patterns.  

### Commands

```powershell
# 1. Filter SMB shares by name pattern (lab example)
Get-SMBShare | Where Name -like '*$*'

# Target-agnostic
Get-SMBShare | Where-Object { $_.Name -like '*<Pattern>*' }
# Example:
# Get-SMBShare | Where-Object { $_.Name -like '*Admin*' }
# Filters network shares containing a specified keyword.


# 2. Display physical disks with a "Healthy" status
Get-PhysicalDisk |
Where-Object -FilterScript { $PSItem.HealthStatus -eq 'Healthy' } |
Select-Object -Property FriendlyName, OperationalStatus

# Target-agnostic
Get-PhysicalDisk |
Where-Object { $_.HealthStatus -eq '<Status>' } |
Select-Object -Property FriendlyName, OperationalStatus
# Example:
# Get-PhysicalDisk | Where-Object { $_.HealthStatus -eq 'Unhealthy' }


# 3. Display extended disk details (formatted)
Get-PhysicalDisk |
Where-Object -FilterScript { $PSItem.HealthStatus -eq 'Healthy' } |
Select-Object -Property FriendlyName, OperationalStatus, DriveLetter, FileSystemLabel, DriveType, FileSystem | fl

# Target-agnostic
Get-PhysicalDisk |
Where-Object { $_.HealthStatus -eq '<Status>' } |
Select-Object -Property FriendlyName, OperationalStatus, DriveLetter, FileSystemLabel, DriveType, FileSystem | Format-List
# The `fl` alias is shorthand for Format-List.


# 4. Filter verbs starting with the letter "C"
Get-Verb | Where { $_.Verb -like 'c*' } | fw

# Target-agnostic
Get-Verb | Where-Object { $_.Verb -like '<Pattern>*' } | Format-Wide
# The `fw` alias stands for Format-Wide, displaying output in multiple columns.
```


## Notes
```powershell
Where-Object filters objects based on a logical condition (true/false).

-like supports wildcards (*pattern*), while -eq, -gt, -lt, etc., perform exact or numeric comparisons.

$_ and $PSItem both reference the current object in the pipeline.

fl and fw are formatting aliases for Format-List and Format-Wide.
```

## Formatting Output

**Purpose:** Control how PowerShell displays data by using formatting cmdlets and property selection to improve readability and presentation.

**Common Scenarios**
- Listing and organizing data for easier review.  
- Formatting service, computer, or user information into readable tables or lists.  
- Automatically adjusting column width to fit the screen.  

### Commands

```powershell
# 1. Display all running services
Get-Service

# Target-agnostic
Get-Service | Select-Object Name, Status
# Retrieves all system services and displays default properties.


# 2. Format service output as a list with selected properties
Get-Service | Format-List -Property Name, Status

# Target-agnostic
Get-Service | Format-List -Property <Property1>, <Property2>
# Example:
# Get-Service | Format-List -Property DisplayName, Status


# 3. Retrieve all computers and include their operating system property
Get-ADComputer -Filter * -Properties OperatingSystem

# Target-agnostic
Get-ADComputer -Filter * -Properties <PropertyName>
# Example:
# Get-ADComputer -Filter * -Properties IPv4Address


# 4. Format AD computer output into a table (ft = Format-Table)
Get-ADComputer -Filter * -Properties OperatingSystem | ft -Property Name, OperatingSystem

# Target-agnostic
Get-ADComputer -Filter * -Properties <PropertyName> |
Format-Table -Property Name, <PropertyName>
# Example:
# Get-ADComputer -Filter * -Properties IPv4Address | Format-Table -Property Name, IPv4Address


# 5. Retrieve all users with all available properties
Get-ADUser -Filter *

# Target-agnostic
Get-ADUser -Filter * -Properties *


# 6. Format AD user output in wide format with automatic column sizing
Get-ADUser -Filter * | fw -AutoSize

# Target-agnostic
Get-ADUser -Filter * | Format-Wide -AutoSize
# The -AutoSize parameter adjusts column width to best fit the data.
```
## Notes
```powershell
Format-Table (ft) displays data in a column-based layout, ideal for multiple properties.

Format-List (fl) displays one property per line for detailed views.

Format-Wide (fw) shows a single property across multiple columns — great for quick overviews.

Formatting cmdlets are for display only — they change how data looks, not the data itself.

Always apply filtering and selection before formatting for best results.
```

## Measuring Objects

**Purpose:** Use `Measure-Object` to calculate counts, totals, averages, minimums, and maximums from PowerShell objects.

**Common Scenarios**
- Counting how many objects are returned from a command.  
- Summing numerical properties (e.g., memory usage, file sizes).  
- Determining averages or statistical summaries for system data.  

### Commands

```powershell
# 1. Count total number of services
Get-Service | Measure-Object

# Target-agnostic
<Command> | Measure-Object
# Example:
# Get-Process | Measure-Object
# Returns a count of all objects passed through the pipeline.


# 2. Count all Active Directory users
Get-ADUser -Filter * | Measure-Object

# Target-agnostic
Get-ADUser -Filter * | Measure-Object
# Example:
# Get-ADComputer -Filter * | Measure-Object
# Use this to quickly verify object totals from AD queries.


# 3. Measure process memory (virtual memory) usage
Get-Process | Measure-Object -Property VM -Sum -Average

# Target-agnostic
<Command> | Measure-Object -Property <PropertyName> -Sum -Average
# Example:
# Get-Process | Measure-Object -Property WorkingSet -Sum -Average
# Calculates total and average memory usage across all running processes.
```
## Notes
```powershell

Measure-Object evaluates numeric properties or counts the total number of objects in the pipeline.

Common parameters:

-Property → Specifies which property to measure.

-Sum, -Average, -Minimum, -Maximum → Perform statistical operations.

Without parameters, Measure-Object defaults to counting objects.

Ideal for quick, in-pipeline summaries without exporting data.
```

## Selecting Objects

**Purpose:** Use `Select-Object` to control which properties of an object are displayed or to select a specific number of results.

**Common Scenarios**
- Displaying only the most relevant object properties.  
- Limiting or grouping results for readability.  
- Sorting and filtering data before output.  

### Commands

```powershell
# 1. Display top 10 processes by memory usage
Get-Process | Sort-Object -Property VM -Descending | Select-Object -First 10

# Target-agnostic
<Command> | Sort-Object -Property <PropertyName> -Descending | Select-Object -First <Count>
# Example:
# Get-ADUser -Filter * | Sort-Object -Property whenCreated -Descending | Select-Object -First 5
# Retrieves the most recent 5 created users.


# 2. Select a specific property from an object
Get-Date | Select-Object -Property DayOfWeek

# Target-agnostic
<Command> | Select-Object -Property <PropertyName>
# Example:
# Get-Service | Select-Object -Property Name, Status
# Extracts only desired columns from output.


# 3. Display the 10 most recent security event log entries
Get-EventLog -Newest 10 -LogName Security | Select-Object -Property EventID, TimeWritten, Message

# Target-agnostic
Get-EventLog -Newest <Count> -LogName <LogName> | Select-Object -Property <Property1>, <Property2>, <Property3>
# Example:
# Get-EventLog -Newest 5 -LogName Application | Select-Object -Property EventID, Source, Message


# 4. Group and display Active Directory computers by operating system
Get-ADComputer -Filter * -Properties OperatingSystem |
Sort-Object -Property OperatingSystem |
Select-Object -Property OperatingSystem, Name |
fl -GroupBy OperatingSystem -Property Name

# Target-agnostic
Get-ADComputer -Filter * -Properties <PropertyName> |
Sort-Object -Property <PropertyName> |
Select-Object -Property <PropertyName>, Name |
Format-List -GroupBy <PropertyName> -Property Name
# Example:
# Group computers by their OS or department.
```
## Notes
```powershell
Select-Object extracts, renames, or limits properties from objects.

# Common parameters:

-Property → Choose which fields to show.

-First, -Last → Limit the number of results.

-Unique → Return distinct values only.

Format-List -GroupBy helps visually separate results based on a shared property (like OperatingSystem).

Combine Sort-Object and Select-Object for precise, filtered reporting.
```

## Sorting Objects

**Purpose:** Use `Sort-Object` to arrange data in ascending or descending order by one or more properties.

**Common Scenarios**
- Organizing service or process lists for easier review.  
- Sorting event logs by time or severity.  
- Presenting Active Directory users alphabetically or by property.  

### Commands

```powershell
# 1. Display all running processes
Get-Process

# Target-agnostic
<Command>
# Example:
# Get-Service
# Lists all items without sorting.


# 2. Sort processes by their process ID
Get-Process | Sort-Object -Property ID

# Target-agnostic
<Command> | Sort-Object -Property <PropertyName>
# Example:
# Get-Process | Sort-Object -Property CPU
# Orders output by the selected property.


# 3. Sort services by status
Get-Service | Sort-Object -Property Status
# Note: “Stopped” appears before “Running” because PowerShell stores status values numerically
# (0 = Stopped, 1 = Running).

# Target-agnostic
<Command> | Sort-Object -Property <PropertyName>
# Example:
# Get-Service | Sort-Object -Property DisplayName


# 4. Sort services by status in descending order
Get-Service | Sort-Object -Property Status -Descending

# Target-agnostic
<Command> | Sort-Object -Property <PropertyName> -Descending
# Example:
# Get-Process | Sort-Object -Property CPU -Descending
# Displays highest or most active processes first.


# 5. Display 10 most recent security event logs
Get-EventLog -LogName Security -Newest 10

# Target-agnostic
Get-EventLog -LogName <LogName> -Newest <Count>
# Example:
# Get-EventLog -LogName Application -Newest 10


# 6. Sort the 10 most recent event logs by time written
Get-EventLog -LogName Security -Newest 10 | Sort-Object -Property TimeWritten

# Target-agnostic
Get-EventLog -LogName <LogName> -Newest <Count> | Sort-Object -Property <PropertyName>
# Example:
# Get-EventLog -LogName Application -Newest 5 | Sort-Object -Property TimeGenerated


# 7. Sort Active Directory users by surname
Get-ADUser -Filter * | Sort-Object -Property SurName | fw -AutoSize

# Target-agnostic
Get-ADUser -Filter * | Sort-Object -Property <PropertyName> | Format-Wide -AutoSize
# Example:
# Get-ADUser -Filter * | Sort-Object -Property Name | Format-Wide -AutoSize
```

## Notes
```powershell
Sort-Object is versatile and works with any pipeline input.

Common parameters:

-Property → Defines the field used for sorting.

-Descending → Reverses the sort order.

You can sort by multiple properties:
Get-Service | Sort-Object -Property Status, DisplayName
```

## Viewing Object Members

**Purpose:** Use `Get-Member` to explore the structure of PowerShell objects — including their properties, methods, and type information.

**Common Scenarios**
- Discovering what information an object contains.  
- Finding which properties you can display or manipulate.  
- Exploring available methods for performing actions on an object.  

### Commands

```powershell
# 1. View members (properties and methods) of service objects
Get-Service | Get-Member

# Target-agnostic
<Command> | Get-Member
# Example:
# Get-Process | Get-Member
# Reveals all properties and methods for the objects returned.


# 2. View members of process objects
Get-Process | Get-Member

# Target-agnostic
<Command> | Get-Member
# Example:
# Get-EventLog -Newest 5 | Get-Member
# Useful for understanding what data each object type exposes.


# 3. View members of file system items
Get-ChildItem | Get-Member

# Target-agnostic
<Command> | Get-Member
# Example:
# Get-Item C:\Windows | Get-Member
# Displays methods and attributes available for file system objects.


# 4. View members of Active Directory user objects
Get-ADUser -Filter * | Get-Member

# Target-agnostic
Get-ADComputer -Filter * | Get-Member
# Useful for discovering built-in AD attributes and methods.


# 5. View all available properties of Active Directory users
Get-ADUser -Filter * -Properties * | Get-Member

# Target-agnostic
Get-ADUser -Filter * -Properties * | Get-Member
# Displays every property for AD users, including extended schema attributes.
```

## Notes
```powershell
Get-Member (alias: gm) inspects and lists members of any PowerShell object.

Member types include:

Property → Static data fields like Name, Status, OperatingSystem.

Method → Actions that objects can perform (e.g., .Kill() for a process).

Event → Triggers or hooks related to object actions.

Common workflow:
<Command> | Get-Member
<Command> | Select-Object -Property <PropertyName>
```