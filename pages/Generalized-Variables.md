

Previous: [Variables with Restricted Values](Variables-with-Restricted-Values.html), Up: [Variables](Variables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 12.17 Generalized Variables

A *generalized variable* or *place form* is one of the many places in Lisp memory where values can be stored using the `setf` macro (see [Setting Generalized Variables](Setting-Generalized-Variables.html)). The simplest place form is a regular Lisp variable. But the CARs and CDRs of lists, elements of arrays, properties of symbols, and many other locations are also places where Lisp values get stored.

Generalized variables are analogous to lvalues in the C language, where ‘`x = a[i]`’ gets an element from an array and ‘`a[i] = x`’ stores an element using the same notation. Just as certain forms like `a[i]` can be lvalues in C, there is a set of forms that can be generalized variables in Lisp.

|                                                                       |    |                            |
| :-------------------------------------------------------------------- | -- | :------------------------- |
| • [Setting Generalized Variables](Setting-Generalized-Variables.html) |    | The `setf` macro.          |
| • [Adding Generalized Variables](Adding-Generalized-Variables.html)   |    | Defining new `setf` forms. |
