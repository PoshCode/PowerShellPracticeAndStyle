# ERR-04 Avoid using $?
When you need to examine the error that occurred, try to avoid using $?. It actually doesn't mean an error did or did not occur; it's reporting whether or not the last-run command considered itself to have completed successfully. You get no details on what happened.
