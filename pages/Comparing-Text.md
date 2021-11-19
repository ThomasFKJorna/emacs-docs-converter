

Next: [Insertion](Insertion.html), Previous: [Buffer Contents](Buffer-Contents.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.3 Comparing Text

This function lets you compare portions of the text in a buffer, without copying them into strings first.

*   Function: **compare-buffer-substrings** *buffer1 start1 end1 buffer2 start2 end2*

    This function lets you compare two substrings of the same buffer or two different buffers. The first three arguments specify one substring, giving a buffer (or a buffer name) and two positions within the buffer. The last three arguments specify the other substring in the same way. You can use `nil` for `buffer1`, `buffer2`, or both to stand for the current buffer.

    The value is negative if the first substring is less, positive if the first is greater, and zero if they are equal. The absolute value of the result is one plus the index of the first differing characters within the substrings.

    This function ignores case when comparing characters if `case-fold-search` is non-`nil`. It always ignores text properties.

    Suppose you have the text ‘`foobarbar haha!rara!`’ in the current buffer; then in this example the two substrings are ‘`rbar `’ and ‘`rara!`’. The value is 2 because the first substring is greater at the second character.

    ```lisp
    (compare-buffer-substrings nil 6 11 nil 16 21)
         ⇒ 2
    ```
