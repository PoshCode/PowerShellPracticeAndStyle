# DOC-07 Don't over-comment
Don't precede each line of code with a comment - doing so breaks up the code and makes it harder to follow. Don't restate the obvious:

````
\# Get the WMI BIOS object from the computer
Get-WmiObject -Class Win32\_BIOS -ComputerName $computer
````
Duh. A well-written PowerShell command, with full command and parameter names, can be pretty self-explanatory. Don't comment-explain it unless it isn't self-explanatory. Comments should describe the logic of your code, not attempt to document what each command is going to do.

Another example:
````
#########################################
#
# PREPARE VARIABLES
#
#########################################

$computers = @()
````
Really? Five lines of comment to one line of functional code? That is not okay. It actually makes the code harder to read. It doesn't add value - anyone looking at the code can tell that a variable is being set up for something. It's just noise.

<table>
<tr><td>![image008.jpg](images/image008.jpg) </td><td>As a note, within the context of the Scripting Games, be really careful about adding help that explains that you took a particular approach because such-and-such other approach wouldn't work. If you write that, you'd better be correct. If you're not, expect to fail without explanation.</td></tr></table>
