

#### 2.4.3.3 Control-Character Syntax

Control characters can be represented using yet another read syntax. This consists of a question mark followed by a backslash, caret, and the corresponding non-control character, in either upper or lower case. For example, both ‘`?\^I`’ and ‘`?\^i`’ are valid read syntax for the character `C-i`, the character whose value is 9.

Instead of the ‘`^`’, you can use ‘`C-`’; thus, ‘`?\C-i`’ is equivalent to ‘`?\^I`’ and to ‘`?\^i`’:

```lisp
?\^I ⇒ 9     ?\C-I ⇒ 9
```

In strings and buffers, the only control characters allowed are those that exist in ASCII; but for keyboard input purposes, you can turn any character into a control character with ‘`C-`’. The character codes for these non-ASCII control characters include the 2\*\*26 bit as well as the code for the corresponding non-control character. Ordinary text terminals have no way of generating non-ASCII control characters, but you can generate them straightforwardly using X and other window systems.

For historical reasons, Emacs treats the `DEL` character as the control equivalent of `?`:

```lisp
?\^? ⇒ 127     ?\C-? ⇒ 127
```

As a result, it is currently not possible to represent the character `Control-?`, which is a meaningful input character under X, using ‘`\C-`’. It is not easy to change this, as various Lisp files refer to `DEL` in this way.

For representing control characters to be found in files or strings, we recommend the ‘`^`’ syntax; for control characters in keyboard input, we prefer the ‘`C-`’ syntax. Which one you use does not affect the meaning of the program, but may guide the understanding of people who read it.
