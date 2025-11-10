Send and Pass Data as Output from the Pipeline
Step 1. Write Pipeline Data to a File

The Out-File cmdlet writes pipeline output to a file.
It captures exactly what would display on-screen and saves it as text.

Get-Service | Out-File -FilePath "ServiceList.txt"

Notes:

Out-File renders objects as text, not as objects.

The result is intended for human reading, not for reuse in scripts.

After Out-File runs, nothing remains in the pipeline (no screen output).

Common Parameters:

-FilePath — specifies the file name or path.

-Append — adds content instead of overwriting.

-Encoding — sets file encoding (UTF8, ASCII, etc.).

Redirection Operators:

PowerShell supports cmd.exe-style redirection:

> → writes (overwrites)

>> → appends

Get-Process > Processes.txt        # Overwrites file
Get-Process >> Processes.txt       # Appends to file

Example Pipeline:
Get-Service |
Sort-Object -Property Status, Name |
Select-Object -Property DisplayName, Status |
Out-File -FilePath ServiceList.csv

Stage	Output Type
Get-Service	System.ServiceProcess.ServiceController
Sort-Object	Same object type (sorted)
Select-Object	Selected.System.ServiceProcess.ServiceController
Out-File	No output — pipeline ends

💡 Debug tip:
If a long pipeline fails, build it step-by-step and verify each command’s output before chaining the next.

Step 2. Convert Pipeline Objects to Other Data Forms

PowerShell can convert or export objects into formats like CSV, XML, or JSON.

Convert vs. Export
Verb	Action	Example
ConvertTo-*	Converts objects and keeps data in memory	ConvertTo-Csv
Export-*	Converts and writes directly to file	Export-Csv
Convert and Export CSV
Get-Service | ConvertTo-Csv | Out-File Services.csv
Get-Service | Export-Csv Services.csv


ConvertTo-Csv → leaves data in memory (for further processing).

Export-Csv → converts + writes to disk.

CSV includes headers (property names).

Import-Csv → re-creates PowerShell objects from CSV files.

Convert and Export XML
Get-Service | Export-Clixml Services.xml


XML is structured and portable, ideal for integration with other applications.

Cmdlet	Description
ConvertTo-Clixml	Converts to XML (keeps in memory)
Export-Clixml	Converts + saves to file

XML preserves hierarchy and supports multi-value properties easily.

Convert to JSON
Get-Service | ConvertTo-Json | Out-File Services.json


JSON is lightweight and widely used for web applications.

PowerShell does not include Export-Json.

Use ConvertTo-Json + Out-File (or > operator) to save.

Convert to HTML
Get-Service | ConvertTo-Html -Title "Service Report" | Out-File Services.html


Creates HTML tables or lists for use in reports or emails.

Useful Parameters:

-Head → content for the <head> section.

-Title → sets <title> tag.

-PreContent / -PostContent → add text before or after the table.

Step 3. Control Additional Output Options
Out-Host

Displays output one page at a time:

Get-ADUser | Out-Host -Paging


This pauses output after each screenful of data.

Out-Printer

Sends pipeline output directly to a printer:

Get-Service | Out-Printer


Or specify a printer name:

Get-Service | Out-Printer -Name "Microsoft Print to PDF"

Out-GridView

Displays data in an interactive GUI window with sort and filter options:

Get-Process | Out-GridView


You can filter columns, copy data, and search dynamically.

Cannot save directly from the grid, but data can be copied or piped elsewhere.

Quick Reference
Cmdlet	Description	Output Type
Out-File	Writes text output to file	None
Export-Csv	Converts objects and writes to CSV	None
Export-Clixml	Converts objects and writes to XML	None
ConvertTo-Json	Converts to JSON (in memory)	String
ConvertTo-Html	Converts to HTML table	String
Out-Host -Paging	Displays output one page at a time	Console
Out-Printer	Sends output to a printer	None
Out-GridView	Displays data in an interactive grid window	GUI
Knowledge Check

Which cmdlet should a user pipe output to for paging results?
➜ Out-Host -Paging

Which symbol overwrites file content using Out-File alias?
➜ >