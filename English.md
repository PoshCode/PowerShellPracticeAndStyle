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

* Use four **spaces** per indentation level (aka soft tabs). No hard tabs.

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

* * Avoid line continuation back tick where not required. In practice, avoid using
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
*
*End each file with a newline.
 

###Script and Function Naming

* For Advanced Functions and scripts use the format of <verb-<noun> for
  naming. For a list of approved verbs the cmdlet `Get-Verb` will list
  them. On the noun side it can be composed of more than one joined word
  using Camel Case and only singular nouns.

* When declaring simple functions leave a space between the function
  name and the parameters.
  
  ```PowerShell
  function MyFunction ($param1, $param2)
  {
    
  }
  ```
* Avoid using the `return` keyword in jur functions, just place the onject 
  variable on its own. In the case of Advanced Functions do not retun values
  inside the `Process {}` block and not in `Begin {}` or `End {}` since it
  defeats the advantage of the pipeline.

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

* Try to include in Advanced Function a OutputType attribute if it returns
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
  

###PowerShell Supported Version

###Loading Third Party .Net Libraries

###Comment Based Help

