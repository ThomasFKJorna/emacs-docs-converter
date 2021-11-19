

Next: [General Escape Syntax](General-Escape-Syntax.html), Up: [Character Type](Character-Type.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 2.4.3.1 Basic Char Syntax

Since characters are really integers, the printed representation of a character is a decimal number. This is also a possible read syntax for a character, but writing characters that way in Lisp programs is not clear programming. You should *always* use the special read syntax formats that Emacs Lisp provides for characters. These syntax formats start with a question mark.

The usual read syntax for alphanumeric characters is a question mark followed by the character; thus, ‘`?A`’ for the character `A`, ‘`?B`’ for the character `B`, and ‘`?a`’ for the character `a`.

For example:

```lisp
?Q ⇒ 81     ?q ⇒ 113
```

You can use the same syntax for punctuation characters. However, if the punctuation character has a special syntactic meaning in Lisp, you must quote it with a ‘`\`’. For example, ‘`?\(`’ is the way to write the open-paren character. Likewise, if the character is ‘`\`’, you must use a second ‘`\`’ to quote it: ‘`?\\`’.

You can express the characters control-g, backspace, tab, newline, vertical tab, formfeed, space, return, del, and escape as ‘`?\a`’, ‘`?\b`’, ‘`?\t`’, ‘`?\n`’, ‘`?\v`’, ‘`?\f`’, ‘`?\s`’, ‘`?\r`’, ‘`?\d`’, and ‘`?\e`’, respectively. (‘`?\s`’ followed by a dash has a different meaning—it applies the Super modifier to the following character.) Thus,

```lisp
?\a ⇒ 7                 ; control-g, C-g
?\b ⇒ 8                 ; backspace, BS, C-h
?\t ⇒ 9                 ; tab, TAB, C-i
?\n ⇒ 10                ; newline, C-j
?\v ⇒ 11                ; vertical tab, C-k
?\f ⇒ 12                ; formfeed character, C-l
?\r ⇒ 13                ; carriage return, RET, C-m
?\e ⇒ 27                ; escape character, ESC, C-[
?\s ⇒ 32                ; space character, SPC
?\\ ⇒ 92                ; backslash character, \
?\d ⇒ 127               ; delete character, DEL
```

These sequences which start with backslash are also known as *escape sequences*, because backslash plays the role of an escape character; this has nothing to do with the character `ESC`. ‘`\s`’ is meant for use in character constants; in string constants, just write the space.

A backslash is allowed, and harmless, preceding any character without a special escape meaning; thus, ‘`?\+`’ is equivalent to ‘`?+`’. There is no reason to add a backslash before most characters. However, you must add a backslash before any of the characters ‘`()[]\;"`’, and you should add a backslash before any of the characters ‘``|'`#.,``’ to avoid confusing the Emacs commands for editing Lisp code. You should also add a backslash before Unicode characters which resemble the previously mentioned ASCII ones, to avoid confusing people reading your code. Emacs will highlight some non-escaped commonly confused characters such as ‘`‘`’ to encourage this. You can also add a backslash before whitespace characters such as space, tab, newline and formfeed. However, it is cleaner to use one of the easily readable escape sequences, such as ‘`\t`’ or ‘`\s`’, instead of an actual whitespace character such as a tab or a space. (If you do write backslash followed by a space, you should write an extra space after the character constant to separate it from the following text.)

Next: [General Escape Syntax](General-Escape-Syntax.html), Up: [Character Type](Character-Type.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
