

### 32.20 Substituting for a Character Code

The following functions replace characters within a specified region based on their character codes.

### Function: **subst-char-in-region** *start end old-char new-char \&optional noundo*

This function replaces all occurrences of the character `old-char` with the character `new-char` in the region of the current buffer defined by `start` and `end`.

If `noundo` is non-`nil`, then `subst-char-in-region` does not record the change for undo and does not mark the buffer as modified. This was useful for controlling the old selective display feature (see [Selective Display](Selective-Display.html)).

`subst-char-in-region` does not move point and returns `nil`.

```lisp
---------- Buffer: foo ----------
This is the contents of the buffer before.
---------- Buffer: foo ----------
```

```lisp
```

```lisp
(subst-char-in-region 1 20 ?i ?X)
     ⇒ nil

---------- Buffer: foo ----------
ThXs Xs the contents of the buffer before.
---------- Buffer: foo ----------
```

### Command: **translate-region** *start end table*

This function applies a translation table to the characters in the buffer between positions `start` and `end`.

The translation table `table` is a string or a char-table; `(aref table ochar)` gives the translated character corresponding to `ochar`. If `table` is a string, any characters with codes larger than the length of `table` are not altered by the translation.

The return value of `translate-region` is the number of characters that were actually changed by the translation. This does not count characters that were mapped into themselves in the translation table.
