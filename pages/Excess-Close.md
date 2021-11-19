

Previous: [Excess Open](Excess-Open.html), Up: [Syntax Errors](Syntax-Errors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 18.3.2 Excess Close Parentheses

To deal with an excess close parenthesis, first go to the beginning of the file, then type `C-u -1 C-M-u` (`backward-up-list` with an argument of -1) to find the end of the first unbalanced defun.

Then find the actual matching close parenthesis by typing `C-M-f` (`forward-sexp`, see [Expressions](https://www.gnu.org/software/emacs/manual/html_node/emacs/Expressions.html#Expressions) in The GNU Emacs Manual) at the beginning of that defun. This will leave you somewhere short of the place where the defun ought to end. It is possible that you will find a spurious close parenthesis in that vicinity.

If you don’t see a problem at that point, the next thing to do is to type `C-M-q` (`indent-pp-sexp`) at the beginning of the defun. A range of lines will probably shift left; if so, the missing open parenthesis or spurious close parenthesis is probably near the first of those lines. (However, don’t assume this is true; study the code to make sure.) Once you have found the discrepancy, undo the `C-M-q` with `C-_` (`undo`), since the old indentation is probably appropriate to the intended parentheses.

After you think you have fixed the problem, use `C-M-q` again. If the old indentation actually fits the intended nesting of parentheses, and you have put back those parentheses, `C-M-q` should not change anything.
