# READ-01 Indent your code
Consider this code example:
````
if ($this -gt $that) {
  Do-Something -with $that
}
````
And now consider this one:
````
if ($this -gt $that)
{
  Do-Something -with $that
}
````
Neither of these is better than the other. Ask 100 coders which they prefer and you'll get roughly half liking either one. Now, when you start dealing with commands that accept script blocks as parameters, things can get trickier because of the way PowerShell parses syntax. "Wrong" is wrong. With scripting constructs, like the two examples above, there's no functional difference.

Continuing in that vein, understand that the following are basically guidelines from mass consensus; they're not hard-and-fast rules. That said, there are arguments in favor of these, and you should consider the arguments before dismissing these ideas.

First, format your code properly. The convention is to indent within constructs, to make it clearer what "belongs to" the construct.
````
ForEach ($computer in $computers) {
 Do-This
 Get-Those
}
````
You will probably be reviled if you don't format carefully.

|  ![scriptgamesimage.jpg](images/scriptgamesimage.jpg) | In the context of the Scripting Games, don't down-score someone for using an approach that you personally don't like to use for aesthetic reasons. If performance is a stated goal, then it's fine to down-score someone for using a technique that's demonstrably slower than an alternative, but never down-grade for personal, subjective, or aesthetic reasons. |
| --- | --- |

