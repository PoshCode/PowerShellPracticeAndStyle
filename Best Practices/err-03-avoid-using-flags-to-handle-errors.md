# ERR-03 Avoid using flags to handle errors

Try to avoid setting flags:

```PowerShell
try {
    $continue = $true
    Do-Something -ErrorAction Stop
} catch {
    $continue = $false
}

if ($continue) {
    Do-This
    Set-That
    Get-Those
}
```

Instead, put the entire "transaction" into the Try block:

```PowerShell
try {
    Do-Something -ErrorAction Stop
    Do-This
    Set-That
    Get-Those
} catch {
    Handle-Error
}
```

It's a lot easier to follow the logic.
