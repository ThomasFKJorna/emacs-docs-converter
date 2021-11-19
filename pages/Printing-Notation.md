

Next: [Error Messages](Error-Messages.html), Previous: [Evaluation Notation](Evaluation-Notation.html), Up: [Conventions](Conventions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 1.3.4 Printing Notation

Many of the examples in this manual print text when they are evaluated. If you execute example code in a Lisp Interaction buffer (such as the buffer `*scratch*`) by typing `C-j` after the closing parenthesis of the example, the printed text is inserted into the buffer. If you execute the example by other means (such as by evaluating the function `eval-region`), the printed text is displayed in the echo area.

Examples in this manual indicate printed text with ‘`-|`’, irrespective of where that text goes. The value returned by evaluating the form follows on a separate line with ‘`⇒`’.

```lisp
(progn (prin1 'foo) (princ "\n") (prin1 'bar))
     -| foo
     -| bar
     ⇒ bar
```
