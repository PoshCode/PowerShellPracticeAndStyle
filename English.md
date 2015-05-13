# The PowerShell Style Guide

## Introduction

In the Python community, developers have a great programming style reference provided as part of the language enhancement process specifications ([PEP-8](https://www.python.org/dev/peps/pep-0008/)), but in the PowerShell world there has been no official documentation of community preferences. 

This document is an attempt to come to an agreement on a style-guide because we know that the more people follow the same set of code-style habits, the more readable the community's code will be. In other words, although the recommendations of this guide are _just recomendations_, if you follow them, you will write PowerShell code that is more easily read, understood, and maintained.

## Table of Contents

<!-- MarkdownTOC depth=4 autolink=true bracket=round -->

- [Code Layout](#code-layout)
    - [Indentation](#indentation)
    - [Maximum Line Length](#maximum-line-length)
    - [Blank lines](#blank-lines)
    - [Trailing spaces](#trailing-spaces)
    - [Spaces around parameters and operators](#spaces-around-parameters-and-operators)
    - [Spaces around special characters](#spaces-around-special-characters)
    - [Avoid using semicolons (`;`) at the end of each line.](#avoid-using-semicolons--at-the-end-of-each-line)
- [Commenting](#commenting)
    - [Block comments](#block-comments)
    - [Inline comments](#inline-comments)
    - [Documentation comments](#documentation-comments)
- [Naming Conventions](#naming-conventions)
    - [Use the full name of each command.](#use-the-full-name-of-each-command)
    - [Use full parameter names.](#use-full-parameter-names)
    - [Use full, explicit paths when possible.](#use-full-explicit-paths-when-possible)
- [Functions](#functions)
- [Advanced Functions](#advanced-functions)
- [Security](#security)
- [PowerShell Supported Version](#powershell-supported-version)
- [Formatting](#formatting)
- [Loading Third Party .Net Libraries](#loading-third-party-net-libraries)
- [Comment Based Help](#comment-based-help)
- [Performance](#performance)
- [Error Handling](#error-handling)

<!-- /MarkdownTOC -->

### Code Layout

Please note that many of these guidelines, in particular, are purely about readability. When we ask you to leave an empty line after a closing function brace, or two lines before functions, we're not being capricious, we're doing so because it makes it easier for experienced developers to scan your code.

#### Indentation

Use four *spaces* per indentation level. This is what PowerShell ISE does and understands, and there's really no excuse good enough to do anything different. 

Do not use tabs except to remain consistent -- when editing code that already uses tabs, you should not change the indentation character for just part of the code, and particularly when contributing to other people's projects, you should never reformat the whitespace on an entire file as _part_ of an edit.

The 4-space rule is optional for continuation lines. Hanging indents (when indenting a wrapped command which was too long) may be indented more than one indentation level, and may be indented an odd number of spaces to line up with a method call or parameter block.

  
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

Limit lines to 110 characters when possible. Although most of us work on large monitors, the PowerShell console is only 120 characters wide, and even when pasting, reserves part of the line for the continuation prompt. 

Keeping files narrower allows for side-by-side editing, so even narrower guidelines (down to the ancient 72 column limit) may be established by various projects, so be sure to check when you're working on someone else's project.

The preferred way to wrap long lines is to use splatting (see [About_Splatting](https://technet.microsoft.com/en-us/library/jj672955.aspx)) and PowerShell's implied line continuation inside parentheses, brackets, and braces -- these should always be used in preference to the backtick for line continuation when applicable.

#### Blank lines

Surround function and class definitions with two blank lines.

Method definitions within a class are surrounded by a single blank line.

Blank lines may be ommitted between a bunch of related one-liners (e.g. empty functions)

Additional blank lines may be used sparingly to separate groups of related functions, or within functions to indicate logical sections (e.g. before a block comment).

End each file with a single blank line.

#### Trailing spaces

Lines should not have trailing whitespace. Extra spaces result in future edits where the only change is a space being added or removed, making the analysis of the changes more difficult for no reason.

#### Spaces around parameters and operators

You should us a single space around parameter names and operators, including comparison operators and math and assignment operators, even when the spaces are not necessary for PowerShell to correctly parse the code.

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
### Commenting

Comments that contradict the code are worse than no comments. Always make a priority of keeping the comments up-to-date when the code changes! 

Comments should be in English, and should be complete sentences, although if they are short, the period at the end can be ommited.

Remember that comments should serve to explain rationale, and (with the exception of regular expressions) the code should be sufficient to explain itself.

```PowerShell
# Do not write:
# Increment Margin by 2
$Margin = $Margin + 2

# Maybe write:
# The rendering box obscures a couple of pixels.
$Margin = $Margin + 2

```

#### Block comments

Block comments generally apply to some or all of the code which follows them, and are indented to the same level as that code. Each line should start with a # and a single space. 

If the block is particularly long (as in the case of documentation text) it is recommended to use the `<# ... #>` block comment syntax, but you should place the comment characters on their own lines, and indent the comment:
  
```PowerShell
  # Requiring a space makes things legible and prevents confusion.
  # Writing comments one-per line makes them stand out more in the console.

  <#  
      .Synopsis
        Really long comment blocks are tedious to keep commented in single-line mode
      .Description
        Particularly when the comment must be frequently edited,
        as with the help and documentation for a function or script
  #>
```

#### Inline comments

Comments on the same line as a statement can be distracting, but when they don't state the obvious, and particularly when you have several short lines of code which need explaining, they can be useful.

They must be separated from the code statement by at least two spaces, and ideally, they should line up with any other inline comments in the same block of code.

```PowerShell
$Options = @{
    Margin = 2          # The rendering box obscures a couple of pixels.
    Padding = 2         # We need space between the border and the text
    FontSize = 24       # Keep this above 16 so it's readable in presentations
}
```

#### Documentation comments

In order to ensure that the documentation stays with the function, documentation comments should be placed INSIDE the function, rather than above. To ensure they stay current with changes to the parameters, they should be placed at the top of the function, above and within the `param` block. Parameter documentation should be within the param block, directly above each parameter:

```PowerShell
function Test-Help {
    <#
        .Synopsis
            An example function to display how help should be written
        .Example
            Get-Help -Name Test-Help

            This shows the help for the example function
    #>
    [CmdletBinding()]
    param(
        # This parameter doesn't do anything.
        # Aliases: MP
        [Parameter(Mandatory=$true)]
        [Alias("MP")]
        [String]$MandatoryParameter
    )

    <# code here ... #>
}
```

### Naming Conventions

In general, prefer the use of full explicit names for commands and parameters rather than aliases or short forms. There are tools [Expand-Alias](https://github.com/PoshCode/ModuleBuilder/blob/master/ResolveAlias.psm1) for fixing many, but not all of these issues.

#### Use the full name of each command.

Every PowerShell scripter learns the actual command names, but different people learn and use different aliases (e.g.: ls for Linux users, dir for DOS users, gci ...).  In your shared scripts you should use the more universally known full command name. As a bonus, sites like GitHub will highlight commands properly when you use the full Verb-Noun name:

```PowerShell
# Do not write:
gwmi -Class win32_service

# Instead write:
Get-WmiObject -Class Win32_Service
```

#### Use full parameter names. 

Because there are so many commands in PowerShell, it's impossible for every scripter to know every command. Therefore it's useful to be explicit about your parameter names for the sake of readers who may be unfamiliar with the command you're using. This will also help you avoid bugs if a future change to the command alters the parameter sets.

```PowerShell
# Do not write:
Get-WmiObject win32_service name,state

# Instead write:
Get-WmiObject -Class win32_service -Property name,state
```

#### Use full, explicit paths when possible.

When writing scripts, it's really only safe to use `..` or `.` in a path if you have previously explicitly set the location (within the script), and even then you should beware of using relative paths when calling .Net methods or legacy/native applications, because they will use the `[Environment]::CurrentDirectory` rather than PowerShell's present working directory (`$PWD`). Because checking for these types of errors is tedious (and because they are easy to over-look) it's best to avoid using relative paths altogether, and instead, base your paths off of $PSScriptRoot (the folder your script is in) when necessary.

```PowerShell
# Do not write:
Get-Content .\README.md

# Especially do not write:
[System.IO.File]::ReadAllText(".\README.md")

# Instead write:
Get-Content (Join-Path $PSScriptRoot README.md)

# Or even use string concatenation:
[System.IO.File]::ReadAllText("$PSScriptRoot\README.md")
```

##### Avoid the use of `~` to represent the home folder. 

The meaning of ~ is unfortunately dependent on the "current" provider at the time of execution. This isn't really a style issue, but it's an important rule for code you intend to share anyway. Instead, use `${Env:UserProfile}` or `(Get-PSProvider FileSystem).Home` ...

```PowerShell
PS C:\Windows\system32> cd ~
PS C:\Users\Name> cd HKCU:\Software
PS HKCU:\Software> cd ~
cd : Home location for this provider is not set. To set the home location, call "(get-psprovider 'Registry').Home = 'path'".
At line:1 char:1
+ cd ~
+ ~~~~
    + CategoryInfo          : InvalidOperation: (:) [Set-Location], PSInvalidOperationException
    + FullyQualifiedErrorId : InvalidOperation,Microsoft.PowerShell.Commands.SetLocationCommand

```

<!-- JOEL STOPPED EDITING HERE -->
 
### Functions
* Avoid using the `return` keyword in your functions. Just place the object 
  variable on its own.

* When declaring simple functions leave a space between the function
  name and the parameters.
  
  ```PowerShell
  function MyFunction ($param1, $param2)
  {
    
  }
  ```

### Advanced Functions

* For advanced functions and scripts use the format of **<verb-<noun>** for
  naming. For a list of approved verbs the cmdlet `Get-Verb` will list
  them. On the noun side it can be composed of more than one joined word
  using Camel Case and only singular nouns.

*  In  Advanced Functions do not use the keyword `return` to return an object.

*  In Advanced Functions you return objects inside the `Process {}` block 
   and not in `Begin {}` or `End {}` since it defeats the advantage of the pipeline.

  ```PowerShell
  
  # Bad
  function Get-USCitizenCapability
  {
      [CmdletBinding()]
      [OutputType([psobject])]
      Param
      (
          [Parameter(Mandatory=$true,
                     ValueFromPipelineByPropertyName=$true,
                     Position=0)]
          [int16]
          $Age
      )
  
      Begin {}
      Process
      {
  
          $Capabilities = @{MilitaryService = $salse
                            DrinkAlcohol = $false
                            Vote = $false}
          if ($Age -ge 18)
          {
              $Capabilities['MilitaryService'] = $true
              $Capabilities['Vote'] = $true
          }
  
          $Obj = New-Object -Property $Capabilities -TypeName psobject
          Return $Obj
      }
      End {}
  }
  
  # Good
   function Get-USCitizenCapability
  {
      [CmdletBinding()]
      [OutputType([psobject])]
      Param
      (
          [Parameter(Mandatory=$true,
                     ValueFromPipelineByPropertyName=$true,
                     Position=0)]
          [int16]
          $Age
      )
  
      Begin {}
      Process
      {
  
          $Capabilities = @{MilitaryService = $false
                            DrinkAlcohol = $false
                            Vote = $false}
          if ($Age -ge 18)
          {
              $Capabilities['MilitaryService'] = $true
              $Capabilities['Vote'] = $true
          }
  
          $Obj = New-Object -Property $Capabilities -TypeName psobject
          $Obj
      }
      End {}
  }
 ```
* For Advanced Functions always use CmdletBinding attribute should always 
  have at least a `Process {}` code block if any parameters takes values
  from the Pipeline.

* When possible use in advanced functions a OutputType attribute if it returns
  an object or collection of objects. If the function returns different
  object types depending on the parameter set provide one per parameter set

  ```PowerShell
  [OutputType([<TypeLiteral>], ParameterSetName="<Name>")]
  [OutputType("<TypeNameString>", ParameterSetName="<Name>")]
  ```

* If ParameterSetName is used in any of the parameter provide always a
  DefaultParameterSetName in the CmdletBinding attribute.
  
  ```PowerShell
   function Get-User
   {
       [CmdletBinding(DefaultParameterSetName="ID")]
  
       [OutputType("System.Int32", ParameterSetName="ID")]
       [OutputType([String], ParameterSetName="Name")]
  
       Param (      
           [parameter(Mandatory=$true, ParameterSetName="ID")]
           [Int[]]
           $UserID,
  
           [parameter(Mandatory=$true, ParameterSetName="Name")]
           [String[]]
           $UserName
       )     
              
     <function body>
   }
  ```
  * When using advanced functions or scripts with CmdletBinding attribute avoid
    validating parameters in the body of the script when possible and use parameter
    validation attributes instead

      * **AllowNull** Validation Attribute

        The AllowNull attribute allows the value of a mandatory parameter
        to be null ($null).
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [AllowNull()]
            [String]
            $ComputerName
          ) 

        ```
      * **AllowEmptyString** Validation Attribute

        The AllowEmptyString attribute allows the value of a mandatory parameter to be
        an empty string ("").
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [AllowEmptyString()]
            [String]
            $ComputerName
          ) 
        ```

      * **AllowEmptyCollection** Validation Attribute

        The AllowEmptyCollection attribute allows the value of a mandatory parameter
        to be an empty collection (@()). 
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [AllowEmptyCollection()]
            [String[]]
            $ComputerName
          ) 
        ```

      * **ValidateCount** Validation Attribute

        The ValidateCount attribute specifies the minimum and maximum number
        of parameter values that a parameter accepts. Windows PowerShell
        generates an error if the number of parameter values in the command that
        calls the function is outside that range. 
        
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [ValidateCount(1,5)]
            [String[]]
            $ComputerName
          ) 

        ```
      * **ValidateLength** Validation Attribute

        The ValidateLength attribute specifies the minimum and maximum number 
        of characters in a parameter or variable value. Windows PowerShell generates an
        error if the length of a value specified for a parameter or a variable
        is outside of the range.
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [ValidateLength(1,10)]
            [String[]]
            $ComputerName
          ) 
        ```
        
      * **ValidatePattern** Validation Attribute

        The ValidatePattern attribute specifies a regular expression that
        is compared to the parameter or variable value. Windows PowerShell generates
        an error if the value does not match the regular expression 
        pattern. 
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [ValidatePattern("[0-9][0-9][0-9][0-9]")]
            [String[]]
            $ComputerName
          ) 
        ```

      * **ValidateRange** Validation Attribute

        The ValidateRange attribute specifies a numeric range for each
        parameter or variable value. Windows PowerShell generates an error
        if any value is outside that range. 
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [ValidateRange(0,10)]
            [Int]
            $Attempts
          ) 
        ```
 

      * **ValidateScript** Validation Attribute

        The ValidateScript attribute specifies a script that is used 
        to validate a parameter or variable value. Windows PowerShell
        pipes the value to the script, and generates an error if the
        script returns "false" or if the script throws an exception.

        When you use the ValidateScript attribute, the value
        that is being validated is mapped to the $_ variable. You can
        use the $_ variable to refer to the value in the script.
        ```PowerShell
        Param
          (
            [parameter()]
            [ValidateScript({$_ -ge (get-date)})]
            [DateTime]
            $EventDate
          ) 
        ```

      * **ValidateSet** Attribute

        The ValidateSet attribute specifies a set of valid values for a 
        parameter or variable. Windows PowerShell generates an error if a
        parameter or variable value does not match a value in the set. In
        the following example, the value of the Detail parameter can only
        be "Low," "Average," or "High."
        
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [ValidateSet("Low", "Average", "High")]
            [String[]]
            $Detail
          ) 
        ```

      * **ValidateNotNull** Validation Attribute

        The ValidateNotNull attribute specifies that the parameter
        value cannot be null ($null). Windows PowerShell generates an
        error if the parameter value is null. 

        The ValidateNotNull attribute is designed to be used when the
        type of the parameter value is not specified or when the specified
        type will accept a value of Null. (If you specify a type that will
        not accept a null value, such as a string, the null value will be
        rejected without the ValidateNotNull attribute, because it does not
        match the specified type.)  
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [ValidateNotNull()]
            $ID
          ) 
        ```

      * **ValidateNotNullOrEmpty** Validation Attribute

        The ValidateNotNullOrEmpty attribute specifies that the parameter 
        value cannot be null ($null) and cannot be an empty string ("").
        Windows PowerShell generates an error if the parameter is used in 
        a function call, but its value is null, an empty string, or an empty
        array.   
        ```PowerShell
        Param
          (
            [parameter(Mandatory=$true)]
            [ValidateNotNullOrEmpty()]
            [String[]]
            $UserName
          ) 
        ```

TODO

* Control what gets exported in a module
* Specify when to use a Manifest for a module

### Security

* Try to use PSCredential object for credentials since by requiring it as a
  parameter it allows for not showing passwords on screen, history or
  exposing password to generic memory scrapper malware.

* If you need to use a credential in clear text like passing it to .Net API call or a third party library it is better to decrypt the credential as it is being passed instead of saving it in a variable.

```PowerShell
# create a credential object.
$cred = (Get-Credential "acmelabs\carlos")

# Get the cleartext username.
$cred.GetNetworkCredential().username

# Get the domain for the account.
$cred.GetNetworkCredential().domain

# Get the cleartext password. 
$cred.GetNetworkCredential().password
```

* For strings that may be sensitive in a parameter use the SecureString type so 
  as to protect the value of the string. When using parameters the string can be
  provided by a user using `Read-Host -AsSecureString` it can be turned to a 
  plain text string back if needed.

```PowerShell
 # Decrypt the secure string.
 $SecureStringToBSTR = [Runtime.InteropServices.Marshal]::SecureStringToBSTR($SecString)
 $PlainText = [Runtime.InteropServices.Marshal]::PtrToStringAuto($SecureStringToBSTR)
```

### PowerShell Supported Version

* When working in an environment where there are multiple versions of PowerShell make sure to specify the lowest version your script will support by prividing a Requires statement at the top of the script.

```PowerShell
#Requires -Version 2.0
```
* When a module uses specific cmdlets or syntax that is only present on a specific minimun version of PowerShell in the module manifest ps1d file.

```PowerShell
PowerShellVersion = '3.0'
```
* If the script or module being created will support as the least 

### Formatting

TODO
* When to use format file.
* why avoid format cmdlets in functions and scripts


### Loading Third Party .Net Libraries

TODO
* Use of Add-Type in PS v2.0 and above instead of reflective loading.
* When embedding C# code makes sense and when it is better to just compile it
 in to a DLL and load it as a Type.
* How to set assemblies in module manifest instead of hand loading

### Comment Based Help

TODO
* leave a newline between comment based help and function declaration
* What should be the least required information in a comment based help
* When to use comment help vs MAML XML files
* When to create about contextual help files

### Performance

TODO
* Proper selection of ForEach method and Foreach-Object Cmdlet
* Give examples of where using .Net API calls is faster than Cmdlets.
* Use of wrapper functions around .Net API
* Give examples for the fastest way to create objects in PS v2.0 and PS v3.0 and above.
* Give example of casting as a faster method
* Give example the where-object is slower than .where() on PS v3.0 and above.

### Error Handling

TODO
* Why avoid `$?`
* Why not rely on $Error[0]
