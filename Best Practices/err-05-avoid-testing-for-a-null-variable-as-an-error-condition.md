# ERR-05 Avoid testing for a null variable as an error condition 

Also try to avoid testing for a null variable as an error condition:

```
$user = Get-ADUser -Identity DonJ

if ($user) {
    $user | Do-Something
} else [
    Write-Warning "Could not get user $user"
}
```

There are times and technologies where that's the only approach that will work, especially if the command you're running won't produce a terminating, trappable exception. But it's a logically contorted approach, and it can make debugging trickier.
