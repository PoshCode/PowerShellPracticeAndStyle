# READ-02 Avoid backticks
Consider this:
````
Get-WmiObject -Class Win32\_LogicalDisk `
       -Filter "DriveType=3" `
       -ComputerName SERVER2
````
In general, the community feels you should avoid using those backticks as "line continuation characters" when possible. They're hard to read, easy to miss, and easy to mis-type. Also, if you add an extra whitespace after the backtick in the above example, then the command won't work. The resulting error is hard to correlate to the actual problem, making debugging the issue harder.

Here's an alternative:
````
$params = @{Class=Win32\_LogicalDisk;
      Filter='DriveType=3';
      ComputerName=SERVER2}

Get-WmiObject @params
````
The technique is called _splatting_. It lets you get the same nice, broken-out formatting without using the backtick. You can also line break after almost any comma, pipe character, or semicolon without using a backtick.

The backtick is not universally hated - but it can be inconvenient. If you have to use it for line breaks, well then, use it. Just try not to have to.
