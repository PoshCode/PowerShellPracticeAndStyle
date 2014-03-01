#The PowerShell Style Guide

##Introduction

In the Python world developers have a great programming style reference (PEP-8), 
on the PowerShell world there is no document to follow officaly from Microsoft, 
but the community it self has come up with several rules they follow. 

The practices in the guide are just recommendations so as to allow a scriptiers
and programers to write PowerShell code that can easyly be maintained in the 
real world and at the same time help enforce the best practices that the 
community has developed.

###Code Layout

* Don't use `;`  at the end of each line. Even do PowerShell will not complain 
it is not C# so there is no need to add it to the line endings.

* Use four **spaces** per indentation level (aka soft tabs). No hard tabs.

    ```PowerShell
    # Good 4 spaces
    if ($Age -gt 21 )
    {
        $Person = "Adult"
    }
    
    # Bad hard Tabs
    if ($Age -gt 21 )
    {
      $Person = "Adult"
    }
    ```
* Do not use Unix-style line endings.

    * If you're using Git you might want to add the following
    configuration setting to protect your project from Unix-Style line
    endings creeping in:

    ```bash
    $ git config --global core.autocrlf false
    ```
*  Use spaces around the `=` operator.

* Use spaces around operators, after commas, colons and semicolons, around `{`
  and before `}`. Whitespace might be (mostly) irrelevant to  PowerShell, 
  but its proper use is the key to writing easily
  readable code.
  
  ```PowerShell
  $Animal = @('dog', 'cat')
  ```
  In the case of `{` and `}` when declaring a hash there should be no space after of before
  but for script blocks it is acceptable since it makes it more readable. 
  
  ```PowerShell
  # Spacing in a hash
  $Numbers = @{one = 1; two = 2, three = 3}

  # Script Block
  Invoke-Command -ScriptBlock { Get-Service -Name BITS }
  ```
  
  In the case of `(` and `)` in string expesions a space is recommended since it makes it more readable.
  
  ```PowerShell
  # Bad Spacing
  $FirstName = 'Carlos'
  'Message' = "Hello $($FirstName)"
  
  # More readable
  $FirstName = 'Carlos'
  'Message' = "Hello $( $FirstName )"
  ```
* No spaces after `(`, `[` or before `]`, `)`.


###Script and Function Naming

###PowerShell Supported Version

###Loading Third Party .Net Libraries

###Comment Based Help

