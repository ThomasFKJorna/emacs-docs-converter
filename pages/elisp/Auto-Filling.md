

### 32.14 Auto Filling

Auto Fill mode is a minor mode that fills lines automatically as text is inserted. See [Auto Fill](https://www.gnu.org/software/emacs/manual/html_node/emacs/Auto-Fill.html#Auto-Fill) in The GNU Emacs Manual. This section describes some variables used by Auto Fill mode. For a description of functions that you can call explicitly to fill and justify existing text, see [Filling](Filling.html).

Auto Fill mode also enables the functions that change the margins and justification style to refill portions of the text. See [Margins](Margins.html).

### Variable: **auto-fill-function**

The value of this buffer-local variable should be a function (of no arguments) to be called after self-inserting a character from the table `auto-fill-chars`, see below. It may be `nil`, in which case nothing special is done in that case.

The value of `auto-fill-function` is `do-auto-fill` when Auto Fill mode is enabled. That is a function whose sole purpose is to implement the usual strategy for breaking a line.

### Variable: **normal-auto-fill-function**

This variable specifies the function to use for `auto-fill-function`, if and when Auto Fill is turned on. Major modes can set buffer-local values for this variable to alter how Auto Fill works.

### Variable: **auto-fill-chars**

A char table of characters which invoke `auto-fill-function` when self-inserted—space and newline in most language environments. They have an entry `t` in the table.

### User Option: **comment-auto-fill-only-comments**

This variable, if non-`nil`, means to fill lines automatically within comments only. More precisely, this means that if a comment syntax was defined for the current buffer, then self-inserting a character outside of a comment will not call `auto-fill-function`.
