# VER-01 Write for the lowest version of PowerShell that you can

As a rule, write for the lowest PowerShell version that you can, especially with scripts that you plan to share with others. Doing so provides the broadest compatibility for other folks.

That said, don't sacrifice functionality or performance just to stick with an older version. If you can safely write for a higher version (meaning you've deployed it everywhere the script will need to run), then take advantage of that version. Keep in mind that some newer features that seem like window dressing might actually have underlying performance benefits. For example, in PowerShell v3:
````
Get-Service | Where-Object -FilterScript { $\_.Status -eq 'Running' }
````
Will run significantly slower than:
````
Get-Service | Where Status -eq Running
````
because of the way the two different syntaxes have to be processed under the hood.

_Further information:_ You can get some detail on the differences between versions of Powershell by typing 'help about\_Windows\_PowerShell\_2.0' (or \_3.0 or 4.0) in Powershell

| ![scriptgamesimage.jpg](images/scriptgamesimage.jpg) | In the context of the Scripting Games, don't ever down-score someone for writing to a particular version, so long as they've documented it, unless the scenario explicitly states that a particular version must be used. |
| --- | --- |



