

### 34.3 Regular Expressions

A *regular expression*, or *regexp* for short, is a pattern that denotes a (possibly infinite) set of strings. Searching for matches for a regexp is a very powerful operation. This section explains how to write regexps; the following section says how to search for them.

For interactive development of regular expressions, you can use the `M-x re-builder` command. It provides a convenient interface for creating regular expressions, by giving immediate visual feedback in a separate buffer. As you edit the regexp, all its matches in the target buffer are highlighted. Each parenthesized sub-expression of the regexp is shown in a distinct face, which makes it easier to verify even very complex regexps.

|                                               |    |                                                 |
| :-------------------------------------------- | -- | :---------------------------------------------- |
| • [Syntax of Regexps](Syntax-of-Regexps.html) |    | Rules for writing regular expressions.          |
| • [Regexp Example](Regexp-Example.html)       |    | Illustrates regular expression syntax.          |
| • [Rx Notation](Rx-Notation.html)             |    | An alternative, structured regexp notation.     |
| • [Regexp Functions](Regexp-Functions.html)   |    | Functions for operating on regular expressions. |
