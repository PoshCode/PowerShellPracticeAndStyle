# WAST-01 Don't re-invent the wheel

There are a number of approaches in PowerShell that will "get the job done." In some cases, other community members may have already written the code to achieve your objectives. If that code meets your needs, then you might save yourself some time by leveraging it, instead of writing it yourself.

For example:

```PowerShell
function Ping-Computer ($computername) {
    $ping = Get-WmiObject Win32_PingStatus -filter "Address='$computername'"
    if ($ping.StatusCode -eq 0) {
        return $true
    } else {
        return $false
    }
}
```

This function has a few opportunities for enhancement. First of all, the parameter block is declared as a basic function; using advanced functions is generally preferred. Secondly, the command verb ("Ping") isn't an approved PowerShell command verb. This will cause warnings if the function is exported as part of a PowerShell module, unless they are otherwise suppressed on import.

Thirdly, there's little reason to write this function in PowerShell v2 or later. PowerShell v2 has a built-in command that will allow you to perform a similar function. Simply use:

```PowerShell
Test-Connection $computername -Quiet
```

This built-in command accomplishes the exact same task with less work on your part - e.g., you don't have to write your own function.

It has been argued by some that, "I didn't know such-and-such existed, so I wrote my own." That argument typically fails with the community, which tends to feel that ignorance is no excuse. Before making the effort to write some function or other unit of code, find out if the shell can already do that. Ask around. And, if you end up writing your own, be open to someone pointing out a built-in way to accomplish it.

On the flip side, it's important to note that writing your own code from the ground up can be useful if you are trying to learn a particular concept, or if you have specific needs that are not offered by another existing solution.
