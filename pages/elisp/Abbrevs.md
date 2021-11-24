

## 36 Abbrevs and Abbrev Expansion

An abbreviation or *abbrev* is a string of characters that may be expanded to a longer string. The user can insert the abbrev string and find it replaced automatically with the expansion of the abbrev. This saves typing.

The set of abbrevs currently in effect is recorded in an *abbrev table*. Each buffer has a local abbrev table, but normally all buffers in the same major mode share one abbrev table. There is also a global abbrev table. Normally both are used.

An abbrev table is represented as an obarray. See [Creating Symbols](Creating-Symbols.html), for information about obarrays. Each abbreviation is represented by a symbol in the obarray. The symbol’s name is the abbreviation; its value is the expansion; its function definition is the hook function for performing the expansion (see [Defining Abbrevs](Defining-Abbrevs.html)); and its property list cell contains various additional properties, including the use count and the number of times the abbreviation has been expanded (see [Abbrev Properties](Abbrev-Properties.html)).

Certain abbrevs, called *system abbrevs*, are defined by a major mode instead of the user. A system abbrev is identified by its non-`nil` `:system` property (see [Abbrev Properties](Abbrev-Properties.html)). When abbrevs are saved to an abbrev file, system abbrevs are omitted. See [Abbrev Files](Abbrev-Files.html).

Because the symbols used for abbrevs are not interned in the usual obarray, they will never appear as the result of reading a Lisp expression; in fact, normally they are never used except by the code that handles abbrevs. Therefore, it is safe to use them in a nonstandard way.

If the minor mode Abbrev mode is enabled, the buffer-local variable `abbrev-mode` is non-`nil`, and abbrevs are automatically expanded in the buffer. For the user-level commands for abbrevs, see [Abbrev Mode](https://www.gnu.org/software/emacs/manual/html_node/emacs/Abbrevs.html#Abbrevs) in The GNU Emacs Manual.

|                                                           |    |                                                                                  |
| :-------------------------------------------------------- | -- | :------------------------------------------------------------------------------- |
| • [Tables](Abbrev-Tables.html)                            |    | Creating and working with abbrev tables.                                         |
| • [Defining Abbrevs](Defining-Abbrevs.html)               |    | Specifying abbreviations and their expansions.                                   |
| • [Files](Abbrev-Files.html)                              |    | Saving abbrevs in files.                                                         |
| • [Expansion](Abbrev-Expansion.html)                      |    | Controlling expansion; expansion subroutines.                                    |
| • [Standard Abbrev Tables](Standard-Abbrev-Tables.html)   |    | Abbrev tables used by various major modes.                                       |
| • [Abbrev Properties](Abbrev-Properties.html)             |    | How to read and set abbrev properties. Which properties have which effect.       |
| • [Abbrev Table Properties](Abbrev-Table-Properties.html) |    | How to read and set abbrev table properties. Which properties have which effect. |
