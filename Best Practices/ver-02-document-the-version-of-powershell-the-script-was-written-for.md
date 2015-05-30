# VER-02 Document the version of PowerShell the script was written for

All that said, make sure you specify the version of PowerShell you wrote for by using an appropriate `#requires` statement:

```PowerShell
#requires -version 3.0
```

The `#requires` statement will prevent the script from running on the wrong version of PowerShell.

