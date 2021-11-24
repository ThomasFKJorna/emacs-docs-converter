

### 18.3 Debugging Invalid Lisp Syntax

The Lisp reader reports invalid syntax, but cannot say where the real problem is. For example, the error ‘`End of file during parsing`’ in evaluating an expression indicates an excess of open parentheses (or square brackets). The reader detects this imbalance at the end of the file, but it cannot figure out where the close parenthesis should have been. Likewise, ‘`Invalid read syntax: ")"`’ indicates an excess close parenthesis or missing open parenthesis, but does not say where the missing parenthesis belongs. How, then, to find what to change?

If the problem is not simply an imbalance of parentheses, a useful technique is to try `C-M-e` (`end-of-defun`, see [Moving by Defuns](https://www.gnu.org/software/emacs/manual/html_node/emacs/Moving-by-Defuns.html#Moving-by-Defuns) in The GNU Emacs Manual) at the beginning of each defun, and see if it goes to the place where that defun appears to end. If it does not, there is a problem in that defun.

However, unmatched parentheses are the most common syntax errors in Lisp, and we can give further advice for those cases. (In addition, just moving point through the code with Show Paren mode enabled might find the mismatch.)

|                                     |    |                                                     |
| :---------------------------------- | -- | :-------------------------------------------------- |
| • [Excess Open](Excess-Open.html)   |    | How to find a spurious open paren or missing close. |
| • [Excess Close](Excess-Close.html) |    | How to find a spurious close paren or missing open. |
