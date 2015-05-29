# The PowerShell Best Practices and Style Guide

These documents are released under the [Creative Commons ShareAlike License](http://creativecommons.org/licenses/by-sa/4.0/). The authors encourage you to redistribute this file as widely as possible, but ask that you send us pull requests with any changes!

The *PowerShell Best Practices* are always evolving, and continue to be edited and updated as the language and tools (and our community understanding of them) evolve. We encourage you to check back for new editions at least twice a year, by visiting https://github.com/PoshCode/PowerShellPracticeAndStyle

The *PowerShell Style Guide* in particular is in PREVIEW, and we are still actively working out our disagreements about the rules in the guide through the github issues system. Please don't be suprised if over then next few weeks we change rules to contradict what they say at this current moment.

#### Contributing

Please use the issues system (and/or GitHub pull requests) to make corrections, contributions, and other changes to the text - we welcome your contributions!

For more information, see [CONTRIBUTING](contributing.md)

#### Credits

_The Community Book of PowerShell Practices_ was originally compiled and edited by Don Jones and Matt Penny with input from the Windows PowerShell community on PowerShell.org

Copyright (c) Don Jones, Matt Penny 2015

_The PowerShell Style Guide_ was originally created by Carlos Perez, for his students, and all the good parts were written by him. 

Copyright (c) Carlos Perez 2015

Any mistakes in either of these documents are there because Joel Bennett got involved. Please submit [issues](https://github.com/PoshCode/PowerShellPracticeAndStyle/issues) and help us correct them.

## Foreword

During the 2013 Scripting Games, it became apparent that a great many folks in the PowerShell community have vastly different ideas about what's "right and wrong" in the world of PowerShell scripting. Some folks down-scored techniques that others praised, and vice-versa.

After the Games, PowerShell.org ran a series of "Great Debate" blog posts, outlining some of the more controversial issues and asking for community input. The catch is the input had to not only state what the person thought was best, but very specifically why they thought that.

The original book, _The Community Book of PowerShell Practices_ was a compilation of those debates' comments, phrased as a "best practices" guide, and based on community input.  It was originally [hosted on Penflip](https://www.penflip.com/powershellorg/the-community-book-of-powershell-practices), but has been turned over to GitHub here in an effort to make it easier to have discussions about the rules.

It's important to realize that neither best practices nor style guides are hard-and-fast rules. We expect a stronger adherence to the syle guide rules than to the best practices, but even then, individual teams or projects may have their own style guidelines which help them to better bridge the various programming languages they work in.

Best practices are the things you usually do as a starting point, and deviate from when it's appropriate. You should understand that these practices were formed primarily by people who are writing scripts to manage their own environment, and who are often pressed for time. They are not usually developing commercial software, and they rarely have the luxury of extended test-and-develop cycles, and for the most part are not professional software developers. 

That perspective has definitely driven what the PowerShell community considers "best" and "worst," and if you are working from a different perspective then you'll have to take all of this with a grain of salt.

The format of the headings was inspired by reading Steven Feuerstein's excellent book 'Oracle PL/SQL Best Practices'.
