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
