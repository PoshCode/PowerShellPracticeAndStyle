# VER-01 Write for as many versions of PowerShell as you can

The advice here has changed much since the advent of PowerShell "Core" and the new [lifecycle of PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/powershell-support-lifecycle?view=powershell-7.1#lifecycle-of-powershell-7).

> Since the open sourcing of PowerShell, PowerShell no longer ships with Windows, which means that "Windows PowerShell 5.1" remains in service long after it should otherwise have been replaced. All versions previous to that are long out of mainstream support (although 5.0 will technically be in "extended" support until at least October of 2023).

Because of this change, we highly recommend that you write code targeting both Windows PowerShell 5.1 and the latest LTS release of PowerShell (as of this writting, that's PowerShell 7.0). Of course, you should also test your code on at least Windows and Linux as well (try WSL!), which means testing in a minimum of three versions (5.1 on Windows, 7.0 on Windows and Linux), and ideally on OS X. It's not too hard with GitHub actions.

Don't sacrifice functionality or performance just to maintain compatibility with an older version. If you can safely write for a higher version in your environment (meaning you've deployed it everywhere the script will need to run), feel free to take advantage of that version. Keep in mind that many features that seem like window dressing actually have underlying performance benefits.

For example, in PowerShell v3 the "simple" syntax for `Where-Object` was introduced, and it's not just cleaner, it's also much faster.

Then in PowerShell v5, the "Where" method was introduced, which is faster still, despite using the old FilterScript syntax.

```PowerShell
Get-Service | Where-Object -FilterScript { $_.Status -eq 'Running' }
```

Runs slower than:

```PowerShell
Get-Service | Where Status -eq Running
```

Runs slower than:

```PowerShell
(Get-Service).Where{ $_.Status -eq 'Running' }
```

# VER-02 Document the versions and platforms of PowerShell that you've tested

All that said, make sure you specify the minimum version of PowerShell you wrote.

For scripts, this means using an appropriate `#requires` statement:

```PowerShell
#requires -version 5.1 -PSEdition Core, Desktop
```

For modules, there are `PowerShellVersion` and `CompatiblePSEditions` properties in the module manifest.

Note that the version and PSEdition are somewhat redundant. The PSEdition can only be "Core" (meaning PowerShell 6+) or "Desktop" (meaning Windows PowerShell), and there's no version of "Desktop" edition higher than 5.1.x, and no version of "Core" lower than 6.0. Worse, you cannot require a specific OS -- except for Windows by requiring `PSEdition` to be `Desktop`. It is possible to use the magic variables `$IsLinux`, `$IsWindows`, and `$IsMacOS` to test for an OS and throw an exception on load, but there is no metadata for that, so most people assume that if you support `PSEdition` = `Core` then you're cross-platform as well.

