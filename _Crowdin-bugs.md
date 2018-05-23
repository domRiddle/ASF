This page is for crowdin test purposes, please do not translate anything here, it'll be removed soon enough.

# Code bug

This string includes a `0`, zero inside a code block. It should be properly rendered by crowdin.

# Escaping bug

The string below is a quote that includes a star symbol, escaped.

> Here is escaped star symbol: \* this text should not be in italic \*

This works, but if we additionally put escaped star in a quote, it's rendered incorrectly:

> Here is escaped star symbol within quotes: "\*" this text should not be in italic "\*"

# Lists bug

Below is a mixed list with number-ordered entries and unordered ones.

1. Number entry 1
2. Number entry 2
* Unordered entry 1
* Unordered entry 2
3. Number entry 3

All of above entries should be kept in-tact, with their newlines preserved.