# ERR-03 Avoid using flags to handle errors

Try to avoid setting flags:
````
Try {
 $continue = $true
 Do-Something -ErrorAction Stop
} Catch {
 $continue = $false
}

if ($continue) {
 Do-This
 Set-That
 Get-Those
}
````
Instead, put the entire "transaction" into the Try block:
````
Try {
 Do-Something -ErrorAction Stop
 Do-This
 Set-That
 Get-Those
} Catch {
 Handle-Error
}
````
It's a lot easier to follow the logic.

