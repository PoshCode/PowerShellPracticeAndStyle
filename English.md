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

* Avoid using aliases for cmdlets and advanced functions since aliases can change
  and make the code hard to read and understand for people not familiar with the
  alias.

* Avoid using positional parameters or abreviations of the parameter names since
  it may make it hard to understand for a person maintaining the code. It may
  introduce bugs in the code since a different parameter set may be in play instead
  of the intended one.

* Comment your code where it makes sense so as to make it more maintenable by
  a third party. Try to avoid over commenting the code.
  
  Over comment:
  ```PowerShell
  # We get the service BITS object to process
  $BitsObj = Get-Service -Name BITS
  ```

* When commenting leave a space between the `#` and the start of the comment.

  ```PowerShell
  #This is a bad style of commenting
  
  # This is more readable
  ```
* If for ever reason the comment needs to span more than 2 lines use a comment 
  block.

  ```PowerShell
    <#  
        This is a very
        long comment block
        that would span more than
        two lines.
    #>
  ```


* Don't use `;`  at the end of each line. Even do PowerShell will not complain 
it is not C# so there is no need to add it to the line endings.

  This is common when declaring hashes where each element is one per line also:
  ```PowerShell
  # Not neccesary the ;
  
  $MyHash = @{one = 1;
            two = 2;
            three = 3;}
            
  # Prefered
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
    
    # Bad hard Tabs
    if ($Age -gt 21)
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
  line continuations. In the case of multiple parameters for a Cmdlet or 
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
  constraints, single indent for the lines after the first is also
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
   
  # Good
  $MyObj.GetData(Param1,
                 Param2,
                 Param3,
                 Param4)
  ```

* Limit lines to 80 characters when possible.

* Avoid trailing whitespace.

* End each file with a newline.
 

###Scripts, Modules, Functions and Advanced Functions
TODO
* When to use eacch and how they differ

####Functions
* Avoid using the `return` keyword in jur functions, just place the onject 
  variable on its own.

* When declaring simple functions leave a space between the function
  name and the parameters.
  
  ```PowerShell
  function MyFunction ($param1, $param2)
  {
    
  }
  ```
####Advanced Functions

* For advanced functions and scripts use the format of **<verb-<noun>** for
  naming. For a list of approved verbs the cmdlet `Get-Verb` will list
  them. On the noun side it can be composed of more than one joined word
  using Camel Case and only singular nouns.

*  In  Advanced Functions do not use the keyword `retun` to retur an object.

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
* For Advanced Functions always use CmdletBinding attribute shoudl always 
  have at least a Process {} code block if any parameters takes values
  from the Pipeline.

* When possible use in advanced functions a OutputType attribute if it returns
  an object or collection of objects. If the function returns different
  object types depending on the parameter set provide one per parameter set

  ```PowerShell
  [OutputType([<TypeLiteral>], ParameterSetName="<Name>")]
  [OutputType("<TypeNameString>", ParameterSetName="<Name>")]
  ```

* If ParameterSetName is used in any of the parameter provide alwats a
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
*
###PowerShell Supported Version
TODO
* Specify prefered version on Scripts.
* Specify prefered version on Modules with a manifest.
* Why not use in a manifest a specified CLR.

###Formating
TODO
* When to use fomat file.
* why avoid format cmdlets in functions and scripts


###Loading Third Party .Net Libraries
TODO
* Use of Add-Type in PS v2.0 and above instead of reflective loading.
* When embeding C# code makes sense and when it is better to just compile it
 in to a DLL and load it as a Type.
* How to set assemblies in module manifest instead of hand loading

###Comment Based Help
TODO
* leave a newline between comment based help and function declaration
* What should be the least required information in a comment based help
* When to use comment help vs MAML XML files
* When to create about contextual help files
###Performance
TODO
* Proper selection of ForEach method and Foreach-Object Cmdlet
* Give examples of where using .Net API calls is faster than Cmdlets.
* Use of wrapper functions around .Net API
* Give examples for the fastes way to create objects in PS v2.0 and PS v3.0 and above.
* Give example of casting as a gaster method
* Give example the where-object is slower than .where() on PS v3.0 and above.

