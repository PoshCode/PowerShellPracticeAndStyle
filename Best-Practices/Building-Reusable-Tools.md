# TOOL-01 Decide whether you're coding a 'command' or a 'script'

For this discussion, it's important to have some agreed-upon terminology. While these terms aren't used universally, and have evolved over time, the PowerShell community generally agrees that at least a couple of different types of powershell code exist.

1. Reusable commands. Sometimes we create (advanced) functions and package them in script modules. Functions are usually small and granular, and perform a single action. Like a PowerShell cmdlet, they can support the pipeline, and output objects which might be passed to other commands. PowerShell modules are the primary way of redistributing reusable commands, as they support packaging, publishing for discovery, and installing.
2. Replayable scripts. Sometimes we create scripts to automate a business processes in a reliable, repeatable way. Scripts leverage functions and cmdlets from PowerShell modules, or even native command-line interface (CLI) tools, but aren't necessarily intended to be reused the same way. Common examples are CI/CD build and deployment scripts, provisioning scripts, and scheduled tasks for maintenance or reporting.

Let's take an example. You might write a "New-ContosoUser.ps1" script to automate part of the on-boarding process for new employees. In it, you might validate business rules about the user's name, call a command like `New-ADUser` to create a user account, enable their email with another command, provision a home folder, and turn on backups, etc. Those discrete tasks might also be used in other processes, so you build them as functions in modules (or you might use commands from modules provided by your email server, or written by third parties). The script, however, is only intended to automate the process of on-boarding new users at your company. It's not "generic" and not necessarily reusable by someone at another company or even another team.

# TOOL-02 Make your code modular

Most working code should be broken down into functions (or compiled "cmdlets") performing a single action, accepting input only via parameters and producing output only as objects of a single type.

# TOOL-03 Make commands as re-usable as possible

When writing script functions or compiled cmdlets, you should accept input only from parameters and should produce any output as objects to the pipeline. This not only makes functions easy to re-use, but also easy to test. When possible, avoid creating or reading global or environment variables.

# TOOL-04 Use PowerShell standard command names

Follow the verb-noun naming convention, using singular nouns and the standard PowerShell verbs.

You can get a list of the verbs by running `Get-Verb` at the command line.

# TOOL-05 Use PowerShell standard parameter naming

Commands should be consistent with PowerShell native cmdlets in regards to parameter naming. This means capitalizing with PascalCase, using singular names, and matching the property name of related objects or similar names on other commands. It also means being sensitive to fact when using your command, people have to type enough to distinguish between parameters, so unique first letters are valued.

For example, credentials are always `$Credential` and not `$UserCreds` or `$Creds`, the server name is always `$ComputerName` even when it accepts a lot of servers, rather than `$ServerName` or `$Servers` or `$Computers` ...

# TOOL-06 Commands should output raw data

The PowerShell community generally agrees that tools should output raw data. That is, the output from internal APIs you're calling should be manipulated as little as possible. If a tool retrieves information represented as a number of bytes, it should output bytes, rather than converting that value to megabytes or some other unit of measure. Having a tool output less-manipulated data helps the tool remain reusable in a larger number of situations, and avoids confusing users who are familiar with the underlying APIs.

# TOOL-07 Scripts may output formatted data

Scripts, on the other hand, may reformat or manipulate data because their goal is not to reusability, and they need only be consistent, and produce human-readable output.

For example, a function named Get-DiskInfo ought to return disk size information in bytes, because that's the very granular unit of measurement that the operating system offers. A script that was creating an inventory of free disk space might only output the formatted gigabyte value, because that unit of measurement is the most convenient for the people who will view the inventory report.

# TOOL-08 Leverage the formatting engine for formatted data

PowerShell supports formatting via configuration in format files. When creating a module, you can specify in the manifest a `.format.ps1xml` file which defines views, and those can manipulate the data and control the default view used by PowerShell to display the output. The format file does not manipulate the underlying data, so the raw data is available for any purpose.

# TOOL-09 Leverage extensibility for enhancing data

PowerShell also supports extending objects via configuration in type files. When creating a module, you can specify in the manifest a `.types.ps1xml` file which defines new properties and methods to be applied to specific types. You can use this when you want to create a whole new property to summarize or reformat information available in the object. See for example the "Mode" column on PowerShell Get-ChildItem output, or the "ParametersString" property on Azures ResourceGroupDeployment objects.

# TOOL-10 Leverage Modules for Distribution

The built-in PowerShellGet commands support publishing and installing to and from not just the public [PowerShellGallery](https://PowerShellGallery.com), but also internal fileshares or private package servers such as [ProGet](https://inedo.com/proget), [Sonatype nexus](https://www.sonatype.com/products/repository-oss), or Azure and Github's NuGet package repositoris (PowerShell packages are NuGet compatible).


# WAST-01 Don't re-invent the wheel

There are a number of approaches in PowerShell that will "get the job done." In some cases, other community members may have already written the code to achieve your objectives. If that code meets your needs, then you can save yourself some time by leveraging it, instead of writing it yourself.

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


# WAST-02 Report bugs to Microsoft (and third party module authors)

An exception: if you know that a built-in technique doesn't work properly (e.g., it is buggy or doesn't accomplish the exact task), then obviously it's fine to re-invent the wheel. However, in cases where you're doing so to avoid a bug or design flaw, then you should - as an upstanding member of the community - report the bug on [github.com/powershell](https://github.com/PowerShell/PowerShell/issues) also.
