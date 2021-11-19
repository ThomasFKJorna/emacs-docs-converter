

Next: [Sequence Type](Sequence-Type.html), Previous: [Character Type](Character-Type.html), Up: [Programming Types](Programming-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 2.4.4 Symbol Type

A *symbol* in GNU Emacs Lisp is an object with a name. The symbol name serves as the printed representation of the symbol. In ordinary Lisp use, with one single obarray (see [Creating Symbols](Creating-Symbols.html)), a symbol’s name is unique—no two symbols have the same name.

A symbol can serve as a variable, as a function name, or to hold a property list. Or it may serve only to be distinct from all other Lisp objects, so that its presence in a data structure may be recognized reliably. In a given context, usually only one of these uses is intended. But you can use one symbol in all of these ways, independently.

A symbol whose name starts with a colon (‘`:`’) is called a *keyword symbol*. These symbols automatically act as constants, and are normally used only by comparing an unknown symbol with a few specific alternatives. See [Constant Variables](Constant-Variables.html).

A symbol name can contain any characters whatever. Most symbol names are written with letters, digits, and the punctuation characters ‘`-+=*/`’. Such names require no special punctuation; the characters of the name suffice as long as the name does not look like a number. (If it does, write a ‘`\`’ at the beginning of the name to force interpretation as a symbol.) The characters ‘`_~!@$%^&:<>{}?`’ are less often used but also require no special punctuation. Any other characters may be included in a symbol’s name by escaping them with a backslash. In contrast to its use in strings, however, a backslash in the name of a symbol simply quotes the single character that follows the backslash. For example, in a string, ‘`\t`’ represents a tab character; in the name of a symbol, however, ‘`\t`’ merely quotes the letter ‘`t`’. To have a symbol with a tab character in its name, you must actually use a tab (preceded with a backslash). But it’s rare to do such a thing.

> **Common Lisp note:** In Common Lisp, lower case letters are always folded to upper case, unless they are explicitly escaped. In Emacs Lisp, upper case and lower case letters are distinct.

Here are several examples of symbol names. Note that the ‘`+`’ in the fourth example is escaped to prevent it from being read as a number. This is not necessary in the sixth example because the rest of the name makes it invalid as a number.

```lisp
foo                 ; A symbol named ‘foo’.
FOO                 ; A symbol named ‘FOO’, different from ‘foo’.
```

```lisp
1+                  ; A symbol named ‘1+’
                    ;   (not ‘+1’, which is an integer).
```

```lisp
\+1                 ; A symbol named ‘+1’
                    ;   (not a very readable name).
```

```lisp
\(*\ 1\ 2\)         ; A symbol named ‘(* 1 2)’ (a worse name).
+-*/_~!@$%^&=:<>{}  ; A symbol named ‘+-*/_~!@$%^&=:<>{}’.
                    ;   These characters need not be escaped.
```

As an exception to the rule that a symbol’s name serves as its printed representation, ‘`##`’ is the printed representation for an interned symbol whose name is an empty string. Furthermore, ‘`#:foo`’ is the printed representation for an uninterned symbol whose name is `foo`. (Normally, the Lisp reader interns all symbols; see [Creating Symbols](Creating-Symbols.html).)

Next: [Sequence Type](Sequence-Type.html), Previous: [Character Type](Character-Type.html), Up: [Programming Types](Programming-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
