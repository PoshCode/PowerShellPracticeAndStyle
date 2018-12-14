# Contributing

Are you interested in helping create the style guide, or do you just want to change a specific rule? You may use the issues system or Github pull requests freely to suggest corrections and simple changes to rules. However, if you want to add a completely new rule, or substantially change a rule, please open an issue and let us discuss it!

## Tone

The points in the best practices documents and the style guide are **practices** and **guidelines**, not rules.

One of our goals, as we continue to improve these documents, is to make it easy to agree with the practices and guidelines in them. Toward that goal, we avoid **absolute** language and prefer **proactive** and **positive** guidelines over negative ones. Specifically, try to avoid words like _always_ and _never_, or _must_ and _forbid_ in favor of words like _usually_, _normally_, _should_ and _avoid_.

The recommendations we provide should make it easier to be successful and harder to fail. They should make it easier for someone else, or even yourself, to pick up the code later. We also explain the rationale, particularly when the reason is readability, maintainability, performance, or security.

Knowing that there are cases where other patterns may be acceptable or even necessary is sufficient reason to allow a practice that is correct most of the time. Each practice or guideline should include a rationale, an example case, and, when appropriate, counterexamples, and notable exceptions.

## Current State

The _PowerShell Best Practices_ are always evolving, and continue to be edited and updated as the language and tools, and our community's understanding of them, evolve. Currently, the existing documents do not always match the guidance in this _Contributing_ guide. We are happy to take edits that, either help us to match our own guidelines, or fulfill the agreed upon outcome of a discussion in the GitHub issues.

## Organization

The guidelines are currently divided into these sections:

- [Style Guide (Introduction)](Style-Guide/Introduction.md)
  - [Code Layout and Formatting](Style-Guide/Code-Layout-and-Formatting.md)
  - [Function Structure](Style-Guide/Function-Structure.md)
  - [Documentation and Comments](Style-Guide/Documentation-and-Comments.md)
  - [Readability](Style-Guide/Readability.md)
  - [Naming Conventions](Style-Guide/Naming-Conventions.md)
- [Best Practices (Introduction)](Best-Practices/Introduction.md)
  - [Naming Conventions](Best-Practices/Naming-Conventions.md)
  - [Building Reusable Tools](Best-Practices/Building-Reusable-Tools.md)
  - [Output and Formatting](Best-Practices/Output-and-Formatting.md)
  - [Error Handling](Best-Practices/Error-Handling.md)
  - [Performance](Best-Practices/Performance.md)
  - [Security](Best-Practices/Security.md)
  - [Language, Interop and .Net](Best-Practices/Language-Interop-and-.Net.md)
  - [Metadata, Versioning, and Packaging](Best-Practices/Metadata-Versioning-and-Packaging.md)

GitHub supports linking within a markdown document, but only to headings. When editing, in addition to keeping practices and guidelines in the document where they make sense, please include a heading for each guideline, and level down heading for rationale, examples, counterexamples, and exceptions.

If you cannot figure out where something should go, open an issue and we will discuss it. If we gather enough guidelines we may need to divide a document by subtopic or open a new section.

Practices and guidelines should have at least a header with an explanatory paragraph.

Style guidelines should be phrased as a prescriptive guideline telling people what to do rather than a being proscriptive or prohibiting rule. They should have both examples and counterexamples.

When you absolutely must write a negative rule, you should start with the phrase _avoid_ and end with an _instead_ sentence, like:

> #### Avoid the use of `~` to represent the home folder
>
> The meaning of `~` is unfortunately dependent on the current provider at the time of execution. This is not really a style issue. It is, however, an important rule for the code you intend to share.
>
> **Instead**, use `${Env:UserProfile}` or `(Get-PSProvider FileSystem).Home`
