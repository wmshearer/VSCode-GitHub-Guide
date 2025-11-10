Filter Objects Out of the Pipeline
Step 1. Comparison Operators in PowerShell

PowerShell uses comparison operators to evaluate objects and determine which ones to keep or remove from the pipeline.
If the comparison returns True, the object continues through the pipeline; if False, it’s filtered out.

Operator	Description	Case-Sensitive Version
-eq	Equal to	-ceq
-ne	Not equal to	-cne
-gt	Greater than	-cgt
-lt	Less than	-clt
-ge	Greater than or equal to	-cge
-le	Less than or equal to	-cle
Wildcard Comparison
Operator	Description
-like	Supports * and ? wildcards
-clike	Case-sensitive version
Other Advanced Operators

-in / -contains → Check if an object exists in a collection.

-as → Tests or converts an object to a specific type.

-match / -cmatch → Compare a string to a regular expression.

Add -not to reverse logic: e.g. -notlike, -notin.

Examples:

100 -gt 10           # True
'hello' -eq 'HELLO'  # True (case-insensitive)
'hello' -ceq 'HELLO' # False (case-sensitive)

Step 2. Basic Filter Syntax

Use Where-Object (alias: Where) to filter objects in the pipeline.

Basic syntax:

Get-Service | Where Status -eq 'Running'


Where (or Where-Object) filters objects by comparing a property to a value.

Case-insensitive by default.

Common Issue:
If a property name is misspelled, PowerShell won’t throw an error — it just returns no results.

Get-Service | Where Stat -eq 'Running'  # Typo! Returns nothing

Limitations of Basic Syntax

Only supports one comparison at a time.

Cannot use nested properties (e.g., .Length).

For complex logic or multiple conditions, use advanced syntax.

Step 3. Advanced Filter Syntax

Advanced syntax uses a filter script block with the -FilterScript parameter.
This gives you full access to the piped object using:

$PSItem (modern PowerShell)

$_ (legacy syntax, still widely used)

Equivalent examples:

# Basic syntax
Get-Service | Where Status -eq 'Running'

# Advanced syntax
Get-Service | Where-Object -FilterScript { $PSItem.Status -eq 'Running' }

# Shorthand with aliases
Get-Service | Where { $_.Status -eq 'Running' }
Get-Service | ? { $_.Status -eq 'Running' }


🧠 Use single quotes around strings like 'Running' to prevent PowerShell from interpreting them as commands.

Combining Multiple Criteria

You can combine conditions with logical operators:

-and

-or

-not

Example:

Get-EventLog -LogName Security -Newest 100 |
Where { $PSItem.EventID -eq 4672 -and $PSItem.EntryType -eq 'SuccessAudit' }


Common Mistakes:

# Missing $PSItem or $_ before second property
Get-Process | Where { $PSItem.CPU -gt 30 -and VM -lt 10000 }   ❌

# Incomplete comparison
Get-Service | Where { $PSItem.Status -eq 'Running' -or 'Starting' }  ❌


✅ Corrected:

Get-Process | Where { $PSItem.CPU -gt 30 -and $PSItem.VM -lt 10000 }
Get-Service | Where { $PSItem.Status -eq 'Running' -or $PSItem.Status -eq 'Starting' }

Filtering True/False Properties

If the property already contains Boolean values (True or False), you can simplify the filter.

Examples:

# Both commands do the same thing
Get-Process | Where { $PSItem.Responding -eq $True }
Get-Process | Where { $PSItem.Responding }

# Reverse logic with -not
Get-Process | Where { -not $PSItem.Responding }

Accessing Nested Properties

Advanced filtering can evaluate sub-properties — something the basic syntax can’t do.

Example: Show services with names longer than 8 characters

Get-Service | Where { $PSItem.Name.Length -gt 8 }

Step 4. Optimize Filter Performance

Filtering efficiently improves performance, especially with large datasets.

Example: Sorting vs Filtering
# Less efficient (sorts everything, then filters)
Get-Block | Sort-Object -Property Letter | Where { $PSItem.Color -eq 'Red' }

# More efficient (filters first, then sorts)
Get-Block | Where { $PSItem.Color -eq 'Red' } | Sort-Object -Property Letter


💡 Rule of thumb:

Filter Left — place filters as far left (early) in the pipeline as possible.

When to Skip Where-Object

Some cmdlets have built-in filtering parameters that perform better than Where-Object.

Example:

# Less efficient
Get-ChildItem | Where { -not $PSItem.PSIsContainer }

# Better option
Get-ChildItem -File


✅ Always check cmdlet help (Get-Help <cmdlet> -Full) for built-in filters before using Where-Object.

Quick Reference
Cmdlet	Purpose	Alias
Where-Object	Filters objects in the pipeline	Where, ?
$PSItem	Represents the current pipeline object (modern)	$_
Filter Left	Optimize performance by filtering early	—
Knowledge Check

Efficient way to list services starting with svc:
➜ Get-Service -Name svc*

Preferred variable for experienced PowerShell users:
$_