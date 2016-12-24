<!-- MarkdownTOC depth=4 autolink=true bracket=round -->
<!-- /MarkdownTOC -->

### Code Layout & Formatting

Please note that many of these guidelines, in particular, are purely about readability. Some of them are arbitrary rules, but they are based on decades of traditions in programming, so while you may disagree with some rules (and should always follow the rules of individual projects), when we ask you to leave an empty line after a closing function brace, or two lines before functions, we're not being capricious, we're doing so because it makes it easier for experienced developers to scan your code.

#### Maintain consistency in layout

Rules about indentation and line length are about consistency across code bases. Long practice has shown that it's easier to read and understand code when it looks familiar and you're not being distracted by details, which means that (as with the python community), it's better for everyone to follow a single set of rules.

We aren't trying to force the whole world to change overnight, and the code layout rules for individual projects trump these rules. Whether for legacy reasons, or to match guidelines for multiple languages in a single project, different projects may have different style guides, and since the goal is consistency, you should always abide by the rules in place on your project.

If you do have a legacy project that is in source control and you decide to reformat code to adopt these rules, try to make all of your whitespace changes in a single a commit that does _nothing_ but edit the whitespace. You should never reformat the whitespace on a file as _part_ of a content change.


#### Always write CmdletBinding

Let's just get this out of the way: all of your scripts should start life as something like this snippet:

```
[CmdletBinding()]param()
begin{}
process{}
end{}
```

In the above example, code in the begin{} block will be executed once, code in the process{} block will execute once per passed parameter, and code in the end{} block will run once each parameter has been passed to process{}.

The CmdletBinding() portion of the code allows you to do four things:
* Enhanced parameters (e.g., mandatory)
* Use Write-Verbose or Write-Debug
* Define -ErrorAction and -ErrorVariable
* Use -WhatIf and -Confirm

#### Open braces on the same line
Code folding is nicer in many editors.
(TODO: This is in discussion in #24)

#### Closing braces always on their own line
Because that's how they're supposed to be!
(TODO: This is in discussion in #24)

#### Prefer: param() begin, process, end
That's the order PowerShell will execute it in
(TODO)


#### Indentation

##### Use four *spaces* per indentation level.

This is what PowerShell ISE does and understands, and it's the default for most code editors. As always, existing projects may have different standards, but for public code, please stick to 4 spaces, and the rest of us will try to do the same.

The 4-space rule is optional for continuation lines. Hanging indents (when indenting a wrapped command which was too long) may be indented more than one indentation level, or may even be indented an odd number of spaces to line up with a method call or parameter block.
  
```PowerShell

# This is ok
$MyObj.GetData(
       $Param1,
       $Param2,
       $Param3,
       $Param4
    )
   
# This is better
$MyObj.GetData($Param1,
               $Param2,
               $Param3,
               $Param4)
```


#### Maximum Line Length

Limit lines to 115 characters when possible. 

The PowerShell console is, by default, 120 characters wide, but it allows only 119 characters on output lines, and when entering multi-line text, PowerShell uses a line continuation prompt: `>>> ` and thus limits your line length to 116 anyway.

Most of us work on widescreen monitors these days, and there is little reason to keep a narrow line width, however, keeping files relatively narrow allows for side-by-side editing, so even narrower guidelines may be established by a given project. Be sure to check when you're working on someone else's project.

The preferred way to avoid long lines is to use splatting (see [About_Splatting](https://technet.microsoft.com/en-us/library/jj672955.aspx)) and PowerShell's implied line continuation inside parentheses, brackets, and braces -- these should always be used in preference to the backtick for line continuation when applicable, even for strings:

```
Write-Host ("This is an incredibly important, and extremely long message. " + 
            "We cannot afford to leave any part of it out, nor do we want line-breaks in the output. " +
            "Using string concatenation let's us use short lines here, and still get a long line in the output")
```

#### Blank lines

Surround function and class definitions with two blank lines.

Method definitions within a class are surrounded by a single blank line.

Blank lines may be ommitted between a bunch of related one-liners (e.g. empty functions)

Additional blank lines may be used sparingly to separate groups of related functions, or within functions to indicate logical sections (e.g. before a block comment).

End each file with a single blank line.

#### Trailing spaces

Lines should not have trailing whitespace. Extra spaces result in future edits where the only change is a space being added or removed, making the analysis of the changes more difficult for no reason.

#### Spaces around parameters and operators

You should use a single space around parameter names and operators, including comparison operators and math and assignment operators, even when the spaces are not necessary for PowerShell to correctly parse the code.

One notable exception is when using semi-colons to pass values to switch parameters:

```PowerShell
# Do not write:
$variable=Get-Content $FilePath -Wai:($ReadCount-gt0) -First($ReadCount*5)

# Instead write:
$variable = Get-Content -Path $FilePath -Wait:($ReadCount -gt 0) -TotalCount ($ReadCount * 5)
```

#### Spaces around special characters

White-space is (mostly) irrelevant to PowerShell, but its proper use is the key to writing easily readable code.

Use a single space after commas and semicolons, and around pairs of curly braces. 

Avoid extra spaces inside parenthesis or square braces.  

Nested expressions `$( ... )` and script blocks `{ ... }` should have a single space _inside_ them to make code stand out and be more readable.

Nested expressions `$( ... )` and variable delimiters `${...}` inside strings do not need spaces _outside_, since that would become a part of the string.
  

#### Avoid using semicolons (`;`) at the end of each line.

PowerShell will not complain about extra semicolons, but they are unecessary, and get in the way when code is being edited or copy-pasted. They also result in extra do-nothing edits in source control when someone finally decides to delete them.

They are also unecessary when declaring hashtables if you are already putting each element on it's own line:

```PowerShell
# This is the preferred way to declare a hashtable if it must go past one line:
$Options = @{
    Margin = 2
    Padding = 2
    FontSize = 24
}  
```
