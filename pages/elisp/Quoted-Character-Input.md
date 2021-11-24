

#### 21.8.5 Quoted Character Input

You can use the function `read-quoted-char` to ask the user to specify a character, and allow the user to specify a control or meta character conveniently, either literally or as an octal character code. The command `quoted-insert` uses this function.

### Function: **read-quoted-char** *\&optional prompt*

This function is like `read-char`, except that if the first character read is an octal digit (0–7), it reads any number of octal digits (but stopping if a non-octal digit is found), and returns the character represented by that numeric character code. If the character that terminates the sequence of octal digits is `RET`, it is discarded. Any other terminating character is used as input after this function returns.

Quitting is suppressed when the first character is read, so that the user can enter a `C-g`. See [Quitting](Quitting.html).

If `prompt` is supplied, it specifies a string for prompting the user. The prompt string is always displayed in the echo area, followed by a single ‘`-`’.

In the following example, the user types in the octal number 177 (which is 127 in decimal).

```lisp
(read-quoted-char "What character")
```

```lisp
---------- Echo Area ----------
What character 1 7 7-
---------- Echo Area ----------

     ⇒ 127
```
