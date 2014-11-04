# The PowerShell Style Guide

## Introduction

In the Python world developers have a great programming style reference (PEP-8)
but there is no equivalent guidance from Microsoft in the PowerShell world. In
its absence, the community has come up with their own best practices.

The guide is written following the power of the "Default" which is to cover
mainly those versions of PowerShell that come pre-installed by default with
Windows (PS v2.0, PS v3.0 and PS v4.0).

The practices in the guide are just recommendations so as to allow scripters
and programers to write PowerShell code that can easily be maintained in the 
real world and at the same time help enforce the best practices that the 
community has developed.
## Table of Contents

* [Code Layout](#code-layout)
* [Security](#security)
* [Functions](#Functions)
* [Advanced Functions](#advanced-functions)
* [PowerShell Supported Version](#powershell-supported-version)
* [Formatting](#formatting)
* [Comment Based Help](#comment-based-help)
* [Performance](#performance)
* [Error Handling](#error-handling)
### Code Layout

* Avoid using aliases for cmdlets and advanced functions since aliases can change
  and make the code hard to read and understand for people not familiar with the
  alias.

  ```PowerShell
  # Bad
  gwmi -Class win32_service

  #Good
  Get-WmiObject -Class Win32_Service
  ```

* Avoid using positional parameters or abbreviations of the parameter names since
  it may make it hard to understand for a person maintaining the code. It may
  introduce bugs in the code since a different parameter set may be in play instead
  of the intended one.
  ```PowerShell
  # Bad
  Get-WmiObject win32_service name,state

  # Good
  Get-WmiObject -Class win32_service -Property name,state
  ```

* Avoid in scripts the use of `~` to represent the home folder since it changes depending on the provider the user is located at the time of execution, also it can be changed representing a whole other path.

  ```PowerShell
  PS C:\Windows\system32> cd ~
  PS C:\Users\Carlos> cd HKCU:\Software
  PS HKCU:\Software> cd ~
  cd : Home location for this provider is not set. To set the home location, call "(get-psprovider 'Registry').Home = 'path'".
  At line:1 char:1
  + cd ~
  + ~~~~
      + CategoryInfo          : InvalidOperation: (:) [Set-Location], PSInvalidOperationException
      + FullyQualifiedErrorId : InvalidOperation,Microsoft.PowerShell.Commands.SetLocationCommand

  ```

* when working with scripts always use full paths instead of relative paths since depending if using cmdlets or .Net API calls may use different paths as their default path. 

* Comment your code where it makes sense so as to make it more maintainable by
  a third party. Try to avoid over commenting the code.
  
  Over comment:
  ```PowerShell
  # We get the service BITS object to process
  # The service is also known as Background Intelligent Transfer Service
  # the service is used for the transfer of files in a intelligent manner
  # in networks where congestion may be a problem.
  $BitsObj = Get-Service -Name BITS
  ```

* When commenting leave a space between the `#` and the start of the comment.

  ```PowerShell
  #This is a bad style of commenting
  
  # This is more readable
  ```
* If a comment needs to span more than two lines use a comment block.

  ```PowerShell
    <#  
        This is a very
        long comment block
        that would span more than
        two lines.
    #>
  ```

* Don't use `;`  at the end of each line. Even though PowerShell will not complain, 
it is not C# so there is no need to add it to the line endings.

  This is common when declaring hashes where each element is one per line also:
  
  ```PowerShell
  # Not necessary the ;
  $MyHash = @{one = 1;
            two = 2;
            three = 3;}
            
  # Preferred
  $MyHash = @{one = 1
            two = 2
            three = 3}
  ```

* Use four **spaces** per indentation level. No hard tabs.

    ```PowerShell
    # Good 4 spaces
    if ($Age -gt 21)
    {
        $Person = "Adult"
    }
    
    # Bad hard Tabs, depending on the application being seen this space can be
    # in 4 spaces or 8 spaces.
    if ($Age -gt 21)
    {
    	$Person = "Adult"
    }
    ```

* Do not use Unix-style line endings.

    * If you're using Git you might want to add the following
    configuration setting to protect your project from Unix-style line
    endings creeping in:

    ```bash
    $ git config --global core.autocrlf false
    ```
*  Use spaces around the `=` operator.

* Use spaces around operators, after commas, colons and semicolons, around `{`
  and before `}`. White-space might be (mostly) irrelevant to PowerShell, 
  but its proper use is the key to writing readable code.
  
  ```PowerShell
  $Animal = @('dog', 'cat')
  ```
  
  In the case of `{` and `}` when declaring a hash there should be no space after of before
  but for script blocks it is acceptable since it makes it more readable. 
  
  ```PowerShell
  # Spacing in a Hash
  $Numbers = @{one = 1; two = 2; three = 3}

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

* Avoid line continuation back tick where not required. In practice, avoid using
  line continuations. In the case of multiple parameters for a cmdlet or 
  function it is better to use a technique called splatting (more info can 
  be obtained by running  `help about_Splatting`

  ```PowerShell
  # Hard to read and troubleshoot
  Get-WmiObject `
      -Class win32_service `
      -Filter "name = 'BITS'" `
      -ComputerName "FS01"
      
  # Easier to read and troubleshoot
  
  $Params = @{Class = win32_service 
              Filter = "name = 'BITS'"
              ComputerName = "FS01"}
  
  Get-WmiObject @Params
  ```
  
* Align the parameters of a method call if they span more than one
  line. When aligning parameters is not appropriate due to line-length
  constraints, single indenting the lines after the first is also
  acceptable.
  
  ```PowerShell
  # Bad double indentation
  $MyObj.GetData(Param1,
      param2,
      Param3,
      Param4)
    

  # Good  
  $MyObj.GetData(
         Param1,
         param2,
         Param3,
         Param4
   )
   
  # Better
  $MyObj.GetData(Param1,
                 Param2,
                 Param3,
                 Param4)
  ```

* Limit lines to 80 characters when possible.

* Avoid trailing whitespace as this could cause in the use of source control software
  to have lines shown as modified where only a space was added or removed making the
  analysis of the changes more difficult in some cases.

* End each file with a newline.
 
#### Functions
* Avoid using the `return` keyword in your functions – just place the object 
  variable on its own.

* When declaring simple functions, leave a space between the function
  name and the parameters.
  
  ```PowerShell
  function MyFunction ($param1, $param2)
  {
    
  }
  ```

#### Advanced Functions

* Use the format of **<verb-<noun>** when naming Advanced Functions or scripts.
  For a list of approved verbs, run `Get-Verb`. Nouns can be composed of multiple
  singular nouns using Camel Case (NounNoun).

*  Do not use `return` to return an object – just place the object variable on its
   own.

*  Return objects inside the `Process {}` block, not in `Begin {}` or `End {}` since
   that defeats the advantage of the pipeline.

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
          $Capabilities = @{MilitaryService = $salse
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
 
* Always use CmdletBinding attribute that has at least a `Process {}` code block
  if any parameters takes values from the Pipeline.

* When possible, use the OutputType attribute if the function returns an object or
  collection of objects. If the function returns different object types depending 
  on the parameter set provide one per parameter set.

  ```PowerShell
  [OutputType([<TypeLiteral>], ParameterSetName="<Name>")]
  [OutputType("<TypeNameString>", ParameterSetName="<Name>")]
  ```

* If ParameterSetName is used in any of the parameter always provide a
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
  
  * When using the CmdletBinding attribute, avoid validating parameters in the body
    of the script when possible and use parameter validation attributes instead

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
            [ValidateScript({$_ -ge (Get-Date)})]
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

* When working in an environment where there are multiple versions of PowerShell, make sure to specify the lowest version your script will support by providing a Requires statement at the top of the script.

```PowerShell
#Requires -Version 2.0
```

* When a module uses specific cmdlets or syntax that is only present on a specific minimun version of PowerShell in the module manifest ps1d file:

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
* Leave a newline between comment based help and function declaration
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
