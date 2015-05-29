# OUT-01 Don't use write-host unless writing to the host is all you want to do
It is generally accepted that you should never use Write-Host to create any script output whatsoever, unless your script (or function, or whatever) uses the Show verb (as in, Show-Performance). That verb explicitly means "show on the screen, with no other possibilities." Like Show-Command.
