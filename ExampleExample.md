## Best Practice: You should usually choose a language feature over a cmdlet 

### Do not: pass a collection through ForEach-Object

    $array | foreach-object { ... } @

#### Except when:

* you are generating output for the pipeline
* you are modifying the objects

#### Instead: use the foreach(){} language construct

    foreach($item in $array) { ... } @

#### Because: 

* You get a named variable, which is easier to read later
* It is faster (around twice as fast)...
  
#### Example:

```
[1]: $array = 1..200KB                 # Duration:  0.01500s 
[2]: $array | ForEach-Object { $null } # Duration: 14.44544s
[3]: foreach($x in $array) { $null }   # Duration:  5.00950s 
```

##### A note about the exception: 

When you are doing a simple operation on a small collection of items and passing that to another cmdlet, or returning the output to a variable, it can be much simpler to pipe through a ForEach-Object because PowerShell does not recognize the foreach(){} loop as having output.  That is, if you wanted to take a list of files and convert their sizes to KB for some reason, you could write either of these commands, but although the first command takes 50% longer on my system, it is less than 0.005 seconds, and it is arguably more legible, and easier to write correctly on the command-line.

```
[4]: $Sizes = Get-ChildItem | ForEach-Object { $_.Length / 1KB }
[5]: $Sizes = foreach( $file in Get-ChildItem ) { $file.Length / 1KB }
```