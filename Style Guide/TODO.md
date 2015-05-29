These documents are in an extremely rough state, not suitable for inclusion in the main guide yet.

<!-- MarkdownTOC depth=4 autolink=true bracket=round -->

- [Always write CmdletBinding](#always-write-cmdletbinding)
  - [Open braces on the same line](#open-braces-on-the-same-line)
  - [Closing braces always on their own line](#closing-braces-always-on-their-own-line)
  - [Prefer: param() begin, process, end](#prefer-param-begin-process-end)
- [Formatting Output](#formatting-output)
  - [Use Format Files for your custom objects](#use-format-files-for-your-custom-objects)
  - [Only output one "kind" of thing at a time](#only-output-one-kind-of-thing-at-a-time)
- [Include Help](#include-help)

<!-- /MarkdownTOC -->


#### Always write CmdletBinding

Let's just get this out of the way: all of your scripts should start life as something like this snippet:

```
[CmdletBinding()]param()
process{}
end{}
```

You can always ignore one of the blocks, and add parameters and such, but you should never write a script without CmdletBinding, and you should never write one without at least considering making it take pipeline input

#### Open braces on the same line
Code folding is nicer in many editors.
(This is in discussion in #24)

#### Closing braces always on their own line
Because that's how they're supposed to be!
(This is in discussion in #24)

#### Prefer: param() begin, process, end
That's the order PowerShell will execute it in


### Formatting Output

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


### Include Help

Discuss: Add a blank line between comment based help and function declaration?
Discuss: Minimum acceptable comment based help: Synopsis, Parameters, and an example for each parameter set (plus pipeline examples if you can contrive one)

TODO: Style guide for MAML help?

