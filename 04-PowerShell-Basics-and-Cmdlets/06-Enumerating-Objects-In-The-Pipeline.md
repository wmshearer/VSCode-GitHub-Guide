Enumerate Objects in the Pipeline
Step 1. Understanding Enumeration

Enumeration is the process of performing an action on each object, one at a time, in a collection.

In many cases, PowerShell automatically handles enumeration for you. For example:

Get-Process -Name Notepad | Stop-Process


is functionally identical to:

Stop-Process -Name Notepad


However, explicit enumeration becomes necessary when:

You need to call a method that has no corresponding cmdlet.

You want to perform custom actions on each object in a pipeline.

Example:
Encrypt all files in a folder using the .Encrypt() method (part of the System.IO.FileInfo object):

Get-ChildItem -File | ForEach-Object -MemberName Encrypt

Step 2. Basic Enumeration Syntax

The ForEach-Object cmdlet (aliases: ForEach, %) is used to enumerate objects.

Basic syntax:
Get-ChildItem -Path C:\Encrypted\ -File | ForEach-Object -MemberName Encrypt


Shorter versions:

Get-ChildItem -Path C:\Encrypted\ -File | ForEach Encrypt
Get-ChildItem -Path C:\Encrypted\ -File | % Encrypt


💡 Note:
Parentheses are not used after the method name in basic syntax.

Limitations

The basic syntax can:

Access only one property or method.

Cannot perform logic (like -and, -or, or comparisons).

Cannot run multiple commands or script blocks.

Example of invalid syntax:

Get-Service | ForEach -MemberName Stop -and -MemberName Close  # ❌ Error

Step 3. Advanced Enumeration Syntax

The advanced form of ForEach-Object gives you full control by running a script block.
This script block executes once for each object in the pipeline.

Example:

Get-ChildItem -Path C:\ToEncrypt\ -File | ForEach-Object -Process { $PSItem.Encrypt() }


Each time through the pipeline:

$PSItem (or $_) refers to the current object.

The script block inside { } runs for each object.

Key Notes

When calling methods in advanced syntax, always include parentheses, even if no parameters exist:

$PSItem.Encrypt()


If the method accepts arguments, place them inside the parentheses, separated by commas.

Step 4. Advanced Enumeration Techniques
1. Using the Range Operator ..

The range operator (..) generates a sequence of integers.
You can use it with ForEach-Object to repeat actions a set number of times.

Example:

1..100 | ForEach-Object { Get-Random }


The range 1..100 generates numbers from 1 to 100.

Each number is passed through the pipeline.

The script block runs 100 times, executing Get-Random each time.

The integer values themselves aren’t used — they simply control repetition.

Quick Reference
Concept	Description	Example
Enumeration	Performing an action on each object in a collection	ForEach-Object
Cmdlet	ForEach-Object	`Get-Service
Aliases	Shortcuts for ForEach-Object	ForEach, %
Range Operator	Creates a sequence of numbers	1..10
Pipeline Variable	Represents the current object	$PSItem or $_
Knowledge Check

Which symbol represents the range operator?
➜ ..

Which symbol represents the alias of the ForEach-Object cmdlet?
➜ %