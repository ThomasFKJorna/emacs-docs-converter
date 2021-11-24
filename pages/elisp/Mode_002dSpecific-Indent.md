

#### 32.17.2 Indentation Controlled by Major Mode

An important function of each major mode is to customize the `TAB` key to indent properly for the language being edited. This section describes the mechanism of the `TAB` key and how to control it. The functions in this section return unpredictable values.

### Command: **indent-for-tab-command** *\&optional rigid*

This is the command bound to `TAB` in most editing modes. Its usual action is to indent the current line, but it can alternatively insert a tab character or indent a region.

Here is what it does:

*   First, it checks whether Transient Mark mode is enabled and the region is active. If so, it calls `indent-region` to indent all the text in the region (see [Region Indent](Region-Indent.html)).
*   Otherwise, if the indentation function in `indent-line-function` is `indent-to-left-margin` (a trivial command that inserts a tab character), or if the variable `tab-always-indent` specifies that a tab character ought to be inserted (see below), then it inserts a tab character.
*   Otherwise, it indents the current line; this is done by calling the function in `indent-line-function`. If the line is already indented, and the value of `tab-always-indent` is `complete` (see below), it tries completing the text at point.

If `rigid` is non-`nil` (interactively, with a prefix argument), then after this command indents a line or inserts a tab, it also rigidly indents the entire balanced expression which starts at the beginning of the current line, in order to reflect the new indentation. This argument is ignored if the command indents the region.

### Variable: **indent-line-function**

This variable’s value is the function to be used by `indent-for-tab-command`, and various other indentation commands, to indent the current line. It is usually assigned by the major mode; for instance, Lisp mode sets it to `lisp-indent-line`, C mode sets it to `c-indent-line`, and so on. The default value is `indent-relative`. See [Auto-Indentation](Auto_002dIndentation.html).

### Command: **indent-according-to-mode**

This command calls the function in `indent-line-function` to indent the current line in a way appropriate for the current major mode.

### Command: **newline-and-indent**

This function inserts a newline, then indents the new line (the one following the newline just inserted) according to the major mode. It does indentation by calling `indent-according-to-mode`.

### Command: **reindent-then-newline-and-indent**

This command reindents the current line, inserts a newline at point, and then indents the new line (the one following the newline just inserted). It does indentation on both lines by calling `indent-according-to-mode`.

### User Option: **tab-always-indent**

This variable can be used to customize the behavior of the `TAB` (`indent-for-tab-command`) command. If the value is `t` (the default), the command normally just indents the current line. If the value is `nil`, the command indents the current line only if point is at the left margin or in the line’s indentation; otherwise, it inserts a tab character. If the value is `complete`, the command first tries to indent the current line, and if the line was already indented, it calls `completion-at-point` to complete the text at point (see [Completion in Buffers](Completion-in-Buffers.html)).

Some major modes need to support embedded regions of text whose syntax belongs to a different major mode. Examples include *literate programming* source files that combine documentation and snippets of source code, Yacc/Bison programs that include snippets of Python or JS code, etc. To correctly indent the embedded chunks, the primary mode needs to delegate the indentation to another mode’s indentation engine (e.g., call `js-indent-line` for JS code or `python-indent-line` for Python), while providing it with some context to guide the indentation. Major modes, for their part, should avoid calling `widen` in their indentation code and obey `prog-first-column`.

### Variable: **prog-indentation-context**

This variable, when non-`nil`, holds the indentation context for the sub-mode’s indentation engine provided by the superior major mode. The value should be a list of the form `(first-column . rest`. The members of the list have the following meaning:

*   `first-column`

    The column to be used for top-level constructs. This replaces the default value of the top-level column used by the sub-mode, usually zero.

*   `rest`

    This value is currently unused.

The following convenience function should be used by major mode’s indentation engine in support of invocations as sub-modes of another major mode.

### Function: **prog-first-column**

Call this function instead of using a literal value (usually, zero) of the column number for indenting top-level program constructs. The function’s value is the column number to use for top-level constructs. When no superior mode is in effect, this function returns zero.
