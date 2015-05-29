# TOOL-01 Decide whether you're coding a 'tool' or a 'controller' script
For this discussion, it's important to have some agreed-upon terminology. While the terminology here isn't used universally, the community generally agrees that several types of "script" exist:

1. Some scripts contain tools, when are meant to be reusable. These are typically functions or advanced functions, and they are typically contained in a script module or in a function library of some kind. These tools are designed for a high level of re-use.
2. Some scripts are controllers, meaning they are intended to utilize one or more tools (functions, commands, etc) to automate a specific business process. A script is not intended to be reusable; it is intended to make use of reuse by leveraging functions and other commands

For example, you might write a "New-CorpUser" script, which provisions new users. In it, you might call numerous commands and functions to create a user account, mailbox-enable them, provision a home folder, and so on. Those discrete tasks might also be used in other processes, so you build them as functions. The script is only intended to automate that one process, and so it doesn't need to exhibit reusability concepts. It's a standalone thing.

Controllers, on the other hand, often produce output directly to the screen (when designed for interactive use), or may log to a file (when designed to run unattended).
