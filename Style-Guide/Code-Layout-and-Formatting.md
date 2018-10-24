### Code Layout & Formatting

These guidelines are about readability. Some of them are arbitrary rules, but they are based on decades of traditions in programming, so while you may disagree with some rules (and should always follow the rules of individual projects), when we ask you to leave an empty line after a closing function brace, or two lines before functions, we're not being capricious, we're doing so because it makes it easier for experienced developers to scan your code.

#### Maintain consistency in layout

Rules about indentation, line length, and capitalization are about consistency across code bases. Long practice has shown that it's easier to read and understand code when it looks familiar and you're not being distracted by details, which means that it's better for everyone in the community to follow a single set of rules.

We don't expect everyone to follow these guidelines, and rules for individual projects always trump these. Whether for legacy reasons, or to match guidelines for multiple languages in a single project, different projects may have different style guidelines. Since the goal is consistency, you should always abide by any style rules that are in place on the project you are contributing to.

If you do have a legacy project that is in source control and you decide to reformat code to adopt these rules, try to make all of your whitespace changes in a single commit that does _nothing_ but edit the whitespace. You should never reformat the whitespace on a file as _part_ of a content change because it makes the changes hard to spot.

#### Capitalization Conventions

PowerShell is **not** case sensitive, but we follow capitalization conventions to make code easy to read. They are based on the [capitalization conventions](https://msdn.microsoft.com/en-us/library/ms229043) Microsoft created for the .NET framework, since PowerShell is a .NET scripting language, and PowerShell cmdlets are primarily written in .NET languages following those guidelines.

##### Terminology

* lowercase - all lowercase, no word separation
* UPPERCASE - all capitals, no word separation
* PascalCase - capitalize the first letter of each word
* camelCase - capitalize the first letter of each word _except_ the first.

PowerShell uses PascalCase for _all_ public identifiers: module names, function or cmdlet names, class, enum, and attribute names, public fields or properties, global variables and constants, etc. In fact, since the _parameters_ to PowerShell commands are actually _properties_ of .Net classes, even parameters use PascalCase rather than camelCase.

PowerShell language keywords are written in lower case (yes, even `foreach` and `dynamicparam`), as well as operators such as `-eq` and `-match`. The keywords in comment-based help are written in UPPERCASE to make it easy to spot them among the dense prose of documentation.

```posh
function Write-Host {
    <#
    .SYNOPSIS
    Writes customized output to a host.
    .DESCRIPTION
    The Write-Host cmdlet customizes output. You can specify the color of text by using
    the ForegroundColor parameter, and you can specify the background color by using the
    BackgroundColor parameter. The Separator parameter lets you specify a string to use to
    separate displayed objects. The particular result depends on the program that is
    hosting Windows PowerShell.
    #>
    [CmdletBinding()]
    param(
        [Parameter(Position = 0, ValueFromPipeline = $true, ValueFromRemainingArguments = $true)]
        [PSObject]
        $Object,

        [Switch]
        $NoNewline,

        [PSObject]
        $Separator,

        [System.ConsoleColor]
        $ForegroundColor,

        [System.ConsoleColor]
        $BackgroundColor
    )
    begin
    {
    ...
```

As stated previously, PowerShell uses PascalCase for _all_ public identifiers. Function names should follow PowerShell's `Verb-Noun` naming conventions, using PascalCase within both Verb and Noun.

A special case is made for two-letter acronyms in which both letters are capitalized, as in the variable `$PSBoundParameters` or the command `Get-PSDrive`. Note that ([as specified in the .NET guidelines](https://msdn.microsoft.com/en-us/library/ms229043#Anchor_1)) this does not affect the commonly capitalized (but not acronym) words "OK" and "ID" . You should also not extend it to compound acronyms, such as when Azure's Resource Manager (RM) meets a Virtual Machine (VM) in `Start-AzureRmVM`...

> We are aware that there are **many** places where these conventions have not been followed properly for various reasons -- you should consider these _exceptions_ (such as for COM interop) or _mistakes_ (such as `System.Data.SqlClient.SQLDebugging`), but not a reason for you to disregard the conventions.

If you wish, you may use camelCase for variables within your functions (or modules) to distinguish _private_ variables from parameters, but this is a matter of taste. Shared variables should be distinguished by using their scope name, such as `$Script:PSBoundParameters` or `$Global:DebugPreference`. If you are using camelCase for a variable that starts with a two-letter acronym (where both letters are capitalized), both letters should be set to lowercase (such as `adComputer`).


#### Always Start With CmdletBinding

All of your scripts or functions should start life as something like this snippet:

```
[CmdletBinding()]param()
process{}
end{}
```

You can always delete or ignore one of the blocks (or add the `begin` block), add parameters and so on, but you should avoid writing scripts or functions without CmdletBinding, and you should always at least _consider_ making it take pipeline input.

#### Brace yourself: Follow the one-true-brace style.
Open braces always go on the same line.  

This style really won in the PowerShell community partly because the style is one of two used in C languages --it's a variant of the K&R (Kernighan and Ritchie) style from their book The C Programming Language-- but also because for the first few years of PowerShell's existence, this was the only style that could be typed at the command line.

Code folding is nicer in many editors when a scriptblock is placed on the end of the same line, as in this example.

````
function Get-Noun {
    end {
        if ($Wide) {
            Get-Command | Sort-Object Noun -Unique | Format-Wide Noun
        } else {
            Get-Command | Sort-Object Noun -Unique | Select-Object -Expand Noun
        }
    }
}
````
#### Closing braces start a new line
Note the above example again, community guidelines recommend following the ['One-True-Brace-Style'](https://www.wikiwand.com/en/Indentation_style#/K&R_style) placing your closing braces on their own line.  This practice makes it easier to pair up matching opening and closing braces when looking to see where a particular scriptblock ends, and allows one to insert new lines of code between any two lines.

To reiterate, these are community best practices, and a lot of the code you'll find online from community leaders will follow these guidelines.  That doesn't mean that those who follow different style guidelines are wrong.  You may be the one to set the course for your company or your own project; we simply offer this guidance for your consideration.

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

The preferred way to avoid long lines is to use splatting (see [About Splatting](https://technet.microsoft.com/en-us/library/jj672955.aspx)) and PowerShell's implied line continuation inside parentheses, brackets, and braces -- these should always be used in preference to the backtick for line continuation when applicable, even for strings:

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

A notable exception is when using colons to pass values to switch parameters:

```PowerShell
# Do not write:
$variable=Get-Content $FilePath -Wai:($ReadCount-gt0) -First($ReadCount*5)

# Instead write:
$variable = Get-Content -Path $FilePath -Wait:($ReadCount -gt 0) -TotalCount ($ReadCount * 5)
```

Another exception is when using [Unary Operators](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators#unary-operators):

```PowerShell
# Do not write:
$yesterdaysDate = (Get-Date).AddDays( - 1)

$i = 0
$i ++

# Instead write:
$yesterdaysDate = (Get-Date).AddDays(-1)

$i = 0
$i++

# Same principle should be applied when using a variable
$yesterdaysDate = (Get-Date).AddDays(-$i)
```

#### Spaces around special characters

White-space is (mostly) irrelevant to PowerShell, but its proper use is the key to writing easily readable code.

Use a single space after commas and semicolons, and around pairs of curly braces.

Avoid extra spaces inside parenthesis or square braces.

Nested expressions `$( ... )` and script blocks `{ ... }` should have a single space _inside_ them to make code stand out and be more readable.

Nested expressions `$( ... )` and variable delimiters `${...}` inside strings do not need spaces _outside_, since that would become a part of the string.


#### Avoid using semicolons (`;`) at the end of each line.

PowerShell will not complain about extra semicolons, but they are unnecessary, and get in the way when code is being edited or copy-pasted. They also result in extra do-nothing edits in source control when someone finally decides to delete them.

They are also unecessary when declaring hashtables if you are already putting each element on it's own line:

```PowerShell
# This is the preferred way to declare a hashtable if it must go past one line:
$Options = @{
    Margin = 2
    Padding = 2
    FontSize = 24
}
```
