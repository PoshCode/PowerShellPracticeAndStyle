### Formatting Output

TODO: This whole document is STILL ROUGH DRAFT

# OUT-01 Don't use write-host unless writing to the host is all you want to do

It is generally accepted that you should never use Write-Host to create any script output whatsoever, unless your script (or function, or whatever) uses the Show verb (as in, Show-Performance). That verb explicitly means "show on the screen, with no other possibilities." Like Show-Command.

# OUT-02 Use write-verbose to give information to someone running your script

Verbose output is generally held to be output that is useful or anyone running the script, providing status information ("now attempting to connect to SERVER1") or progress information ("10% complete").

# OUT-03 Use write-debug to give information to someone maintaining your script

Debug output is generally held to be output that is useful for script debugging ("Now entering main loop," "Result was null, skipping to end of loop"), since it also creates a breakpoint prompt.

# OUT-04 Use [CmdletBinding()] if you are using write-debug or write-verbose

Both Verbose and Debug output are off by default, and when you use Write-Verbose or Write-Debug, it should be in a script or function that uses the `[CmdletBinding()]` declaration, which automatically enables the switch.

The CmdletBinding attribute is specified on the first line of the script or function. After the name and inline help, but before the parameter definition:

```Powershell
function your-function {
    <#
        Comment-based help
    #>
    [CmdletBinding()]
    param([String]$Parameter)
```




#### Use Format Files for your custom objects

Never use format commands in functions. Instead you should include a `modulename.format.ps1xml` file from your module's PSD1, and define a PSTypeName on your objects, so that PowerShell will format your output automatically.

In a few rare conditions, it might be ok to output custom text to the host (e.g. via Write-Host), or a graphical UI:

* If you have a -Formatted switch parameter
* If you have a command which uses the verb `Show`

#### Only output one "kind" of thing at a time

When possible, you should avoid mixing output types, and you should indicate with an attribute the output type of a script cmdlet.

Particularly, avoid outputting strings interspersed with your output just to avoid Write-Host.

When you combine multiple objects, they should be derived from a common basetype (like FileInfo and DirectoryInfo come from System.IO.FileSystemInfo), or should have format files or type files which cause them to output the same columns.

Otherwise, you may get empty rows of output, etc.

**Exception:** For internal functions, it's ok to return arrays where each item is a different type, meant for assignment like so:

```PowerShell
$user, $group, $account = Get-UserGroupAccoutStatus
```

These get an exemption because they're not intended to ever be "output" to the host, and can offer significant savings (e.g. one database call with three table joins, instead of three database calls with two or three joins each).

When you do output multiple object types, you should name your function in such a way that it's obvious to users that you're returning multiple things.

Remember that PowerShell buffers output, so if you must output different types, you should call Out-Default once for each type to make sure they come out separately.
