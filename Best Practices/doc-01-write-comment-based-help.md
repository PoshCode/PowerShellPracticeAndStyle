# DOC-01 Write comment-based help

You should always write comment-based help in your scripts and functions.

Comment-based help is formatted as follows:

```PowerShell
function get-example {
<#
.SYNOPSIS
A brief description of the function or script.

.DESCRIPTION
A longer description.

.PARAMETER FirstParameter
Description of each of the parameters

.PARAMETER SecondParameter
Description of each of the parameters

.INPUTS
Description of objects that can be piped to the script

.OUTPUTS
Description of objects that are output by the script

.EXAMPLE
Example of how to run the script

.LINK
Links to further documentation

.NOTES
Detail on what the script does, if this is needed

#>
```

Comment-based help is displayed when the user types `help get-example` or `get-example -?`, etc.

Your help should be helpful. That is, if you've written a tool called `Get-LOBAppUser`, don't write help that merely says, "Gets LOB App Users." Duh.

**Further information:** You can get more on the use of comment-based help by typing `help about_Comment_Based_Help` within Powershell.
