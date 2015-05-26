PSStyleGuide
============

The Community PowerShell Style Guide

# Status of This Guide

The PowerShell Style Guide is in PREVIEW.

We are still actively working out our disagreements about the rules in this guide, so please don't be suprised if, over then next few weeks we change rules to contradict what they say at this current moment.

## Contributing

Markdown documents on GitHub support linking to any header, so when editing, please observe the following conventions:

1. Keep rules within the section where they make sense.
2. Sections must be header level 3 (have three hashes): `### Naming Conventions`
3. Rules must be headers with an explanatory paragraph
5. Rules should have examples and counter examples
6. Rules should be phrased as the positive, rather than the negative.
  * Don't say: "Avoid using aliases"
  * Do say: "Use full command names"
7. When writing a negative rule, you should always start with "avoid" and end with an "instead" sentence, like:<blockquote><h6>Avoid the use of `~` to represent the home folder.</h6><p>The meaning of ~ is unfortunately dependent on the "current" provider at the time of execution. This isn't really a style issue, but it's an important rule for code you intend to share anyway. <em>Instead</em>, use `${Env:UserProfile}` or `(Get-PSProvider FileSystem).Home`</p></blockquote>

