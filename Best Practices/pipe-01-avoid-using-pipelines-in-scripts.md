# PIPE-01 Avoid using pipelines in scripts

Consider this:

```PowerShell
Get-Content computer-names.txt |
ForEach-Object -Process {
 Get-WmiObject -Class Win32\_BIOS -ComputerName $\_
}
```

And now this alternative:

```PowerShell
$computers = Get-Content computer-names.txt

foreach ($computer in $computers) {
 Get-WmiObject -Class Win32\_BIOS -ComputerName $computer
}
```

The world definitely prefers the latter approach in a script or function.

Constructs offer far more flexibility, especially when you're looping through multiple commands. Error handling becomes far easier and more structured with the latter approach, as does debugging.

Constructs often exhibit higher performance than pipeline-only approaches, but not always -  it's worth testing if you are dealing with large data-sets and if you're in a context where performance matters. See PERF-02 for more detail on this.

On the command-line, by yourself, do whichever you prefer. It's as you move into a script - an inherently more structured environment - that you want to start shifting to a more programming-style approach.

## One-Liners

When PowerShell scripters talk about a "one-liner," they mean a single, continuous pipeline. Using semicolons to jam multiple "lines" into a single "line" does not count. If someone is asking for a one-liner, they're doing it to test a certain type of PowerShell mettle. Mashing multiple lines together by means of semicolons does not count, but wrapping your pipeline for readability doesn't necessarily count against you either.

## "The PowerShell Way"

There is no one, true way to do anything in PowerShell. To resurrect a previous pair of examples:

```PowerShell
[void]Do-Something

Do-Something | Out-Null
```

Same end effect, but some folks will argue that the first command is more "programmer-y" and less "PowerShell-y." The first example runs faster, however, so there's more than an aesthetic difference here.

It's important to note that although PowerShell isn't a programming language, it - like most good shells - does contain a programming language. It's okay to use it. It's fine to use more programming-style approaches. Don't dismiss a technique or approach because 'it looks like programming' to you, and because you aren't a programmer and don't want to be one. You might actually be missing out on something.
