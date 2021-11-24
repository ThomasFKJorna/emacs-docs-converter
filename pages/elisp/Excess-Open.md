

#### 18.3.1 Excess Open Parentheses

The first step is to find the defun that is unbalanced. If there is an excess open parenthesis, the way to do this is to go to the end of the file and type `C-u C-M-u` (`backward-up-list`, see [Moving by Parens](https://www.gnu.org/software/emacs/manual/html_node/emacs/Moving-by-Parens.html#Moving-by-Parens) in The GNU Emacs Manual). This will move you to the beginning of the first defun that is unbalanced.

The next step is to determine precisely what is wrong. There is no way to be sure of this except by studying the program, but often the existing indentation is a clue to where the parentheses should have been. The easiest way to use this clue is to reindent with `C-M-q` (`indent-pp-sexp`, see [Multi-line Indent](https://www.gnu.org/software/emacs/manual/html_node/emacs/Multi_002dline-Indent.html#Multi_002dline-Indent) in The GNU Emacs Manual) and see what moves. **But don’t do this yet!** Keep reading, first.

Before you do this, make sure the defun has enough close parentheses. Otherwise, `C-M-q` will get an error, or will reindent all the rest of the file until the end. So move to the end of the defun and insert a close parenthesis there. Don’t use `C-M-e` (`end-of-defun`) to move there, since that too will fail to work until the defun is balanced.

Now you can go to the beginning of the defun and type `C-M-q`. Usually all the lines from a certain point to the end of the function will shift to the right. There is probably a missing close parenthesis, or a superfluous open parenthesis, near that point. (However, don’t assume this is true; study the code to make sure.) Once you have found the discrepancy, undo the `C-M-q` with `C-_` (`undo`), since the old indentation is probably appropriate to the intended parentheses.

After you think you have fixed the problem, use `C-M-q` again. If the old indentation actually fit the intended nesting of parentheses, and you have put back those parentheses, `C-M-q` should not change anything.
