Understand the Windows PowerShell Pipeline
Step 1. Review Windows PowerShell Pipeline and Its Output

A pipeline is a chain of one or more commands where the output of one command becomes the input for the next.

Commands in a pipeline are separated by the pipe symbol | and executed left to right.

Each command line you enter is a single pipeline — PowerShell processes it, displays the output, and waits for the next command.

Example:

Get-Service | Where-Object {$_.Status -eq 'Running'} | Select-Object Name, DisplayName


Get-* commands often provide input for Set-* commands.

Where and Select (aliases for Where-Object and Select-Object) filter or select object properties.

Objects in PowerShell

PowerShell doesn’t output text — it outputs objects, which are structured pieces of data with properties (attributes) and methods (actions).

Think of a command’s output like a spreadsheet:

Rows → individual objects

Columns → object properties

Example:

Get-Service


Outputs a collection of service objects with properties such as Name, DisplayName, and Status.

Why this matters:
Unlike text-based shells, PowerShell eliminates the need for parsing text — it directly passes structured data (objects) between commands.

Step 2. Discover Object Members in PowerShell

Objects contain members, including:

Properties – Describe attributes (like Name, Status, or ID).

Methods – Perform actions (like Stop(), Start(), or Clear()).

Events – Trigger when something happens (like file updates or process completion).

Use Get-Member to see all members of an object:

Get-Service | Get-Member


Alias: gm

Displays all properties, methods, and the type name of the object (for example, System.ServiceProcess.ServiceController).

⚠️ Tip: Be careful when piping commands that change system configurations—avoid combining -WhatIf with Get-Member, since it suppresses output.

Step 3. Control the Formatting of Pipeline Output

PowerShell automatically formats output, but you can control it using formatting cmdlets:

Cmdlet	Description	Alias
Format-List	Displays each property on a new line (best for detailed output).	fl
Format-Table	Displays data in a table (best for comparing multiple objects).	ft
Format-Wide	Displays a single property in multiple columns (compact).	fw
Format-Custom	Uses XML for custom layouts (advanced use).	—
Format-List Example
Get-Process | Format-List


Each property is displayed on its own line.

Ideal for commands returning many properties.

Format-Table Example
Get-ADObject -Filter * -Properties * | ft -Property Name, ObjectClass, Description -AutoSize -Wrap


Displays data in a structured table with automatic column sizing.

Common parameters:

-AutoSize adjusts column width.

-HideTableHeaders removes headers.

-Wrap wraps long text.

Format-Wide Example
Get-GPO -All | fw -Property DisplayName -Column 3


Displays a single property (like names) across multiple columns.

Perfect for large, simple lists.

Quick Reference
Concept	Description
Pipeline	Transfers command output as input to the next command.
Object	Structured data with properties and methods.
Get-Member	Displays all members (properties/methods) of an object.
Format-List	Displays properties line-by-line.
Format-Table	Displays properties in columns.
Format-Wide	Displays one property in multiple columns.
Knowledge Check

What is a pipeline in PowerShell?
➜ A chain of one or more commands where the output from one command passes as input to the next.

Which formatting cmdlet displays each property on a new line?
➜ Format-List