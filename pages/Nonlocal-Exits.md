

Previous: [Generators](Generators.html), Up: [Control Structures](Control-Structures.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 11.7 Nonlocal Exits

A *nonlocal exit* is a transfer of control from one point in a program to another remote point. Nonlocal exits can occur in Emacs Lisp as a result of errors; you can also use them under explicit control. Nonlocal exits unbind all variable bindings made by the constructs being exited.

|                                               |    |                                                      |
| :-------------------------------------------- | -- | :--------------------------------------------------- |
| • [Catch and Throw](Catch-and-Throw.html)     |    | Nonlocal exits for the program’s own purposes.       |
| • [Examples of Catch](Examples-of-Catch.html) |    | Showing how such nonlocal exits can be written.      |
| • [Errors](Errors.html)                       |    | How errors are signaled and handled.                 |
| • [Cleanups](Cleanups.html)                   |    | Arranging to run a cleanup form if an error happens. |
