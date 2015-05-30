# ERR-02 Use $ErrorActionPreference='Stop' or 'Continue' when calling non-cmdlets

When executing something other than a cmdlet, set $ErrorActionPreference='Stop' before executing, and re-set to Continue afterwards. If you're concerned about using -ErrorAction because it will bail on the entire pipeline, then you've probably over-constructed the pipeline. Consider using a more scripting-construct-style approach, because those approaches are inherently better for automated error handling.

Ideally, whatever command or code you think might bomb should be dealing with one thing: querying one computer, deleting one file, updating one user. That way, if an error occurs, you can handle it and then get on with the next thing.

