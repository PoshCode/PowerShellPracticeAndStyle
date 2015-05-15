These documents are in an extremely rough state, not suitable for inclusion in the main guide yet.

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
