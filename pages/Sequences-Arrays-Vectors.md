

Next: [Records](Records.html), Previous: [Lists](Lists.html), Up: [Top](index.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

## 6 Sequences, Arrays, and Vectors

The *sequence* type is the union of two other Lisp types: lists and arrays. In other words, any list is a sequence, and any array is a sequence. The common property that all sequences have is that each is an ordered collection of elements.

An *array* is a fixed-length object with a slot for each of its elements. All the elements are accessible in constant time. The four types of arrays are strings, vectors, char-tables and bool-vectors.

A list is a sequence of elements, but it is not a single primitive object; it is made of cons cells, one cell per element. Finding the `n`th element requires looking through `n` cons cells, so elements farther from the beginning of the list take longer to access. But it is possible to add elements to the list, or remove elements.

The following diagram shows the relationship between these types:

```lisp
          _____________________________________________
         |                                             |
         |          Sequence                           |
         |  ______   ________________________________  |
         | |      | |                                | |
         | | List | |             Array              | |
         | |      | |    ________       ________     | |
         | |______| |   |        |     |        |    | |
         |          |   | Vector |     | String |    | |
         |          |   |________|     |________|    | |
         |          |  ____________   _____________  | |
         |          | |            | |             | | |
         |          | | Char-table | | Bool-vector | | |
         |          | |____________| |_____________| | |
         |          |________________________________| |
         |_____________________________________________|
```

|                                                 |    |                                                |
| :---------------------------------------------- | -- | :--------------------------------------------- |
| • [Sequence Functions](Sequence-Functions.html) |    | Functions that accept any kind of sequence.    |
| • [Arrays](Arrays.html)                         |    | Characteristics of arrays in Emacs Lisp.       |
| • [Array Functions](Array-Functions.html)       |    | Functions specifically for arrays.             |
| • [Vectors](Vectors.html)                       |    | Special characteristics of Emacs Lisp vectors. |
| • [Vector Functions](Vector-Functions.html)     |    | Functions specifically for vectors.            |
| • [Char-Tables](Char_002dTables.html)           |    | How to work with char-tables.                  |
| • [Bool-Vectors](Bool_002dVectors.html)         |    | How to work with bool-vectors.                 |
| • [Rings](Rings.html)                           |    | Managing a fixed-size ring of objects.         |

Next: [Records](Records.html), Previous: [Lists](Lists.html), Up: [Top](index.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
