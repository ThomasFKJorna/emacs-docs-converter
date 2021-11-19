

Next: [Hash Table Type](Hash-Table-Type.html), Previous: [Char-Table Type](Char_002dTable-Type.html), Up: [Programming Types](Programming-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 2.4.11 Bool-Vector Type

A *bool-vector* is a one-dimensional array whose elements must be `t` or `nil`.

The printed representation of a bool-vector is like a string, except that it begins with ‘`#&`’ followed by the length. The string constant that follows actually specifies the contents of the bool-vector as a bitmap—each character in the string contains 8 bits, which specify the next 8 elements of the bool-vector (1 stands for `t`, and 0 for `nil`). The least significant bits of the character correspond to the lowest indices in the bool-vector.

```lisp
(make-bool-vector 3 t)
     ⇒ #&3"^G"
(make-bool-vector 3 nil)
     ⇒ #&3"^@"
```

These results make sense, because the binary code for ‘`C-g`’ is 111 and ‘`C-@`’ is the character with code 0.

If the length is not a multiple of 8, the printed representation shows extra elements, but these extras really make no difference. For instance, in the next example, the two bool-vectors are equal, because only the first 3 bits are used:

```lisp
(equal #&3"\377" #&3"\007")
     ⇒ t
```
