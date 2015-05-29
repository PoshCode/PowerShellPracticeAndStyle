# PERF-01 If performance matters, test it
PowerShell comes equipped with 3.2 million performance quirks. Approximately.

For example, the first line below executes a lot faster than the second:
````
[void]Do-Something
Do-Something | Out-Null
````
If you're aware of multiple techniques to accomplish something, and you're writing a production script that will be dealing with large data sets (meaning performance will become a cumulative factor), then test the performance using Measure-Command or some other tool.

|  ![scriptgamesimage.jpg](images/scriptgamesimage.jpg) | In the context of the Scripting Games, if performance is a stated goal, then it's fine to down-score someone for using a technique that's demonstrably slower than an alternative, but never down-grade for personal, subjective, or aesthetic reasons. |
| --- | --- |
