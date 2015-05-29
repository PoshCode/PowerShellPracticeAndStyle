# ERR-06 Copy $Error[0] to your own variable
Within a Catch block, $\_ will contain the last error that occurred, as will $Error[0]. Use either - but immediately copy them into your own variable, as executing additional commands can cause $\_ to get "hijacked" or $Error[0] to contain a different error.

It isn't necessary to clear $Error in most cases. $Error[0] will be the last error, and PowerShell will maintain the rest of the $Error collection automatically.
